# Bayes.PM.Sim
# This version gets the values into the empty list kappa.mean.output by
# making sure they are type consistent 
#Need to source all three of these things
source("DBDA2E-utilities.R") # Load definitions of graphics functions etc.
source("BernBeta.R") # Load the definition of the BernBeta function
#To get the mean from the prior data
#Produces the choice of 1 or 0 for a piece of evidence
x = c(1,0)
#Samples from x 10 and replicates this ten times with prob of 1 = .3 and 0 = .7
#It produces 10 sets of data with 10 data points each
#It produces a different set of numbers each time
x.sample = replicate(10, sample(x, 10, replace = TRUE, prob = c(.4, .6))); x.sample
x.sample.m = matrix(x.sample, 10, 10); x.sample.m
#Gets the means for the rows, because you want the mean of the student's data over time
x.sample.m.a = apply(x.sample.m, 1, mean); x.sample.m.a
# Changing the name for convience 
mean.prior = x.sample.m.a; mean.prior
#######################################################################################################
######################################################################################################
# Creating the for loop to create the length of evidence, the prob of success and failure
# This version just has one for loop for the evidence, success, and failure
x = c(1,0)
Bayes.Sim.n = c(5, 10, 15); Bayes.Sim.n
Bayes.Sim.success = c(2, 5, 8); Bayes.Sim.success
Bayes.Sim.success.percent = Bayes.Sim.success / 10; Bayes.Sim.success.percent 
Bayes.Sim.failure.percent = 1-Bayes.Sim.success.percent; Bayes.Sim.failure.percent

test = function(){
  for (i in Bayes.Sim.n){
    for(j in Bayes.Sim.success.percent){
      for(k in Bayes.Sim.failure.percent){
    x.sample = replicate(10, sample(x, i, replace = TRUE, prob = c(j, k)))
    print(x.sample)
      } 
    }
  }
}  
test()
n.rep.m = matrix(n.rep, 10, 10); n.rep
#Gets the means for the columns
n.rep.m.a = round(apply(n.rep.m, 2, mean), 0); n.rep.m.a
# Changing the name for convience
n.prior = n.rep.m.a; n.prior
###########################################################################################################
# Here I am changing the shape parameters 
a = mean.prior*n.prior; mean.prior; a
b = (1-mean.prior)*n.prior; n.prior; b
###############################################################################################################
# Here is code for calculating the posterior mean that is correct
a = 5
b = 5
Data = c(rep(1,1),rep(0,9)); Data
z = sum( Data ) # number of 1's in Data
N = length( Data ) 
Theta = seq(0.001,0.999,by=0.001) # points for plotting
pThetaGivenData = dbeta( Theta , a+z , b+N-z ); pThetaGivenData  # posterior for plotting
post.mean = (z+a)/(N +a+b); post.mean
