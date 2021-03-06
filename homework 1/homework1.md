#### Write functions that implement the L1 loss and tilted absolute loss functions.

    # L2 loss function
    L2_loss <- function(y, yhat, tau){
      (y-yhat)^2}
    # L1_loss function
    L1_loss <- function(y, yhat, tau){
      abs(y-yhat)}
    # L1 tilted
    L1_tilted_loss <- function(y, yhat, tau){
      res <- c()
      for(i in 1:length(y)){
        if((y[i] - yhat[i]) > 0){res[i] <- tau*(y[i] - yhat[i])}
        if((y[i] - yhat[i]) <= 0){res[i] <- (tau - 1)*(y[i] - yhat[i])}
      }
      return(res)}

#### Create a figure that shows lpsa (x-axis) versus lcavol (y-axis). Add and label (using the 'legend' function) the linear model predictors associated with L2 loss, L1 loss, and tilted absolute value loss for tau = 0.25 and 0.75.

    library(ElemStatLearn)

    ## load prostate data
    data(prostate)
    ## subset to training examples
    prostate_train <- subset(prostate, train=TRUE)

    ## fit simple linear model using numerical optimization (from prostate-data-lin.R)
    fit_lin <- function(y, x, tau, loss=L2_loss, beta_init = c(-0.51, 0.75)) {
      # create the training error function
      err <- function(beta){
        # average loss on the training data
        mean(loss(y,  beta[1] + beta[2]*x, tau))
      }
      # optim by default minimises the fn argumenrt based on the par
      beta <- optim(par = beta_init, fn = err)
      return(beta)
    }
    ## make predictions from linear model using the beta we fit to the data (from prostate-data-lin.R)
    predict_lin <- function(x, beta){
      beta[1] + beta[2]*x
    }
      

    # compute parameters that minimises the loss function
    lin_beta_L2 <- fit_lin(y=prostate_train$lcavol, x=prostate_train$lpsa, loss=L2_loss, tau = NULL)
    lin_beta_L1 <- fit_lin(y=prostate_train$lcavol, x=prostate_train$lpsa, loss=L1_loss, tau = NULL)
    lin_beta_tiltedL1_025 <- fit_lin(y=prostate_train$lcavol, x=prostate_train$lpsa, loss=L1_tilted_loss, tau = 0.25)
    lin_beta_tiltedL1_075 <- fit_lin(y=prostate_train$lcavol, x=prostate_train$lpsa, loss=L1_tilted_loss, tau = 0.75)

    # compute predicted values for each loss function
    x_grid <- seq(min(prostate_train$lpsa), max(prostate_train$lpsa), length.out=100)
    lin_pred_L2 <- predict_lin(x = x_grid, beta = lin_beta_L2$par)
    lin_pred_L1 <- predict_lin(x = x_grid, beta = lin_beta_L1$par)
    lin_pred_tiltedL1_025 <- predict_lin(x = x_grid, beta = lin_beta_tiltedL1_025$par)
    lin_pred_tiltedL1_075 <- predict_lin(x = x_grid, beta = lin_beta_tiltedL1_075$par)

    # plot the values from the dataset
    plot(prostate_train$lpsa, prostate_train$lcavol, xlab="log Prostate Screening Antigen (psa)", 
         ylab="log Cancer Volume (lcavol)", ylim = c(-2, 5))
    # add lines for the predicted values
    lines(x = x_grid, y = lin_pred_L2, col = "black")
    lines(x = x_grid, y = lin_pred_L1, col = "red")
    lines(x = x_grid, y = lin_pred_tiltedL1_025, col = "blue")
    lines(x = x_grid, y = lin_pred_tiltedL1_075, col = "green")

    # add legend to the plot
    legend("bottomright", legend=c("L2 loss", "L1 loss", "Tilted L1 (tau = 0.25)", "Tilted L1 (tau = 0.75)"),
           col=c("black", "red", "blue", "green"), lty = 1, cex=0.8)

![](homework1_files/figure-markdown_strict/unnamed-chunk-2-1.png)

#### Write functions to fit and predict from a simple exponential (nonlinear) model with three parameters defined by *b**e**t**a*\[1\]+*b**e**t**a*\[2\]×*e**x**p*(−*b**e**t**a*\[3\]×*x*). Hint: make copies of 'fit\_lin' and 'predict\_lin' and modify them to fit the nonlinear model. Use c(-1.0, 0.0, -0.3) as 'beta\_init'.

    ## fit simple nonlinear model using numerical optimization
    fit_nonlin <- function(y, x, tau, loss, beta_init = c(-1.0, 0.0, -0.3)) {
      # create the training error function
      err <- function(beta){
        # average loss on the training data
        mean(loss(y,  beta[1] + beta[2]*exp(-beta[3]*x), tau))
      }
      # optim by default minimises the fn argumenrt based on the par
      beta <- optim(par = beta_init, fn = err, control=list(maxit= 1e4))
      return(beta)
    }
    ## make predictions from linear model using the beta we fit to the data
    predict_nonlin <- function(x, beta){
      beta[1] + beta[2]*exp(-beta[3]*x)
    }

#### Create a figure that shows lpsa (x-axis) versus lcavol (y-axis). Add and label (using the 'legend' function) the nonlinear model predictors associated with L2 loss, L1 loss, and tilted absolute value loss for tau = 0.25 and 0.75.

    # compute parameters that minimises the loss function
    nonlin_beta_L2 <- fit_nonlin(y=prostate_train$lcavol, x=prostate_train$lpsa, loss=L2_loss, tau = NULL)
    nonlin_beta_L1 <- fit_nonlin(y=prostate_train$lcavol, x=prostate_train$lpsa, loss=L1_loss, tau = NULL)
    nonlin_beta_tiltedL1_025 <- fit_nonlin(y=prostate_train$lcavol, x=prostate_train$lpsa, loss=L1_tilted_loss, tau = 0.25)
    nonlin_beta_tiltedL1_075 <- fit_nonlin(y=prostate_train$lcavol, x=prostate_train$lpsa, loss=L1_tilted_loss, tau = 0.75)

    # compute predicted values for each loss function
    x_grid <- seq(min(prostate_train$lpsa), max(prostate_train$lpsa), length.out=100)
    nonlin_pred_L2 <- predict_nonlin(x = x_grid, beta = nonlin_beta_L2$par)
    nonlin_pred_L1 <- predict_nonlin(x = x_grid, beta = nonlin_beta_L1$par)
    nonlin_pred_tiltedL1_025 <- predict_nonlin(x = x_grid, beta = nonlin_beta_tiltedL1_025$par)
    nonlin_pred_tiltedL1_075 <- predict_nonlin(x = x_grid, beta = nonlin_beta_tiltedL1_075$par)

    # plot the values from the dataset
    plot(prostate_train$lpsa, prostate_train$lcavol, xlab="log Prostate Screening Antigen (psa)", 
         ylab="log Cancer Volume (lcavol)", ylim = c(-2, 5))
    # add lines for the predicted values
    lines(x = x_grid, y = nonlin_pred_L2, col = "black")
    lines(x = x_grid, y = nonlin_pred_L1, col = "red")
    lines(x = x_grid, y = nonlin_pred_tiltedL1_025, col = "blue")
    lines(x = x_grid, y = nonlin_pred_tiltedL1_075, col = "green")

    # add legend to the plot
    legend("bottomright", legend=c("L2 loss", "L1 loss", "Tilted L1 (tau = 0.25)", "Tilted L1 (tau = 0.75)"),
           col=c("black", "red", "blue", "green"), lty = 1, cex=0.8)

![](homework1_files/figure-markdown_strict/unnamed-chunk-4-1.png)
