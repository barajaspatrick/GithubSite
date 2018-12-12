---
layout : post
title: Linear Model Optimization Algorithm
---

Setup
=====

Our goal for this exercise is to pick the best features that predict MPG
from the cars dataset. Our first step is to load the dataset and isolate
the feature columns and predictor columns, and from there create a list
of the features names.

    library(datasets)
    ## select all columns except the "mpg" column
    cars.df <- mtcars[, -which(names(mtcars) == "mpg")]
    cars.df <- names(cars.df)
    cars.df ## confirm we selected all except 'mpg' col

    ##  [1] "cyl"  "disp" "hp"   "drat" "wt"   "qsec" "vs"   "am"   "gear" "carb"

Feature Combinations
--------------------

Using the list of names we created we will now run it through a loop
that will list of all the possible combinations we can make using our
list. The output of our loop will be a list of matrixies where in each
matrix the column represents the predictors in our linear model. Each
matrix contains progressivy more and more predictors; for example:
matrix one contains 1 row with 10 columns for each one of our
predictors, matrix two contains 2 rows with 45 cols for each of the
possible combinations of two predictors and so on.

    combinationList <- list()
    ## This loop will generate a 'list' of matrixes
    for (i in 1:length(cars.df)){
            temp <- combn(cars.df, i)
            combinationList[[i]] <- temp
    }
    rm(list = ("temp"))

Model Loops
-----------

our next step is to extract the elements from our list of matrixes and
create a single list containing all the possible predictors we can use
in our model. Our next step will be to convert the variable lists into a
format our Linear Model function can read. The last step will be to loop
our linear model function through all the variable list

    LinearModels.df <- NULL
    for (i in 1:length(combinationList)){
            current_matrix <- combinationList[[i]]  
            for (j in 1:ncol(current_matrix)){

                    ## vector of covariates used in the model.
                    cVariableList <- (current_matrix[,j])

                    ## format variables for our model
                    variables <- paste(cVariableList, collapse = "+")
                    form <- as.formula(paste("mpg", variables, sep = "~"))

                    tempModel <- lm(form, data = mtcars)
                    ## returns r squared
                    R_Squared <- summary(tempModel)[[8]]
                    ## returns adjusted r squared
                    adjusted_R_Squared <- summary(tempModel)[[9]]

                    LinearModels.df <- rbind(LinearModels.df,
                                             data.frame(variables, R_Squared,
                                                        adjusted_R_Squared))
            }
    }
    rm(list = c("combinationList", "current_matrix", "tempModel"))
    head(LinearModels.df, 5)

    ##   variables R_Squared adjusted_R_Squared
    ## 1       cyl 0.7261800          0.7170527
    ## 2      disp 0.7183433          0.7089548
    ## 3        hp 0.6024373          0.5891853
    ## 4      drat 0.4639952          0.4461283
    ## 5        wt 0.7528328          0.7445939

    tail(LinearModels.df, 5)

    ##                                     variables R_Squared adjusted_R_Squared
    ## 1019      cyl+disp+hp+wt+qsec+vs+am+gear+carb 0.8675709          0.8133953
    ## 1020    cyl+disp+drat+wt+qsec+vs+am+gear+carb 0.8629415          0.8068721
    ## 1021      cyl+hp+drat+wt+qsec+vs+am+gear+carb 0.8655375          0.8105301
    ## 1022     disp+hp+drat+wt+qsec+vs+am+gear+carb 0.8689448          0.8153314
    ## 1023 cyl+disp+hp+drat+wt+qsec+vs+am+gear+carb 0.8690158          0.8066423

Results
-------

Once we have all the data collected from our Linear Model loop we can
create a pair of functions to find the R-squared, and Adjusted R-Squared
values for each of the model we ran.

    ## will find the row with the maximum adjusted_r col
    findMaxAdjRS <- function(){
            MaxAdjR_Square.sub <- subset(LinearModels.df, adjusted_R_Squared ==
                                            max(LinearModels.df$adjusted_R_Squared))
            MaxAdjR_Square.sub
    }

    findMaxRS <- function(){
            MaxR_Square.sub <- subset(LinearModels.df, R_Squared ==
                                            max(LinearModels.df$R_Squared))
            MaxR_Square.sub
    }

Finaly we can run our two functions and find the best combination of
variables that predict MPG.

    ## note: adjusted r squared gives a penalty for every
    ## additional predictor included in model this to avoid overfitting.
    findMaxAdjRS()  

    ##              variables R_Squared adjusted_R_Squared
    ## 528 disp+hp+wt+qsec+am 0.8637377          0.8375334

    findMaxRS()

    ##                                     variables R_Squared adjusted_R_Squared
    ## 1023 cyl+disp+hp+drat+wt+qsec+vs+am+gear+carb 0.8690158          0.8066423

    #disp+hp+wt+qsec+am

Summary
-------

From our results we can summise that the the best model is
"mpg~disp+hp+wt+qsec+am" with a adjusted R-Squared of .837. in
otherwords our model is capable of explaining ~84% of the variation in
MPG.
