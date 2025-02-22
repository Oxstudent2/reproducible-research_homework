# Reproducible research: version control and R

\# INSERT ANSWERS HERE #

link to logistic growth repo and answers to questions 1,2,3:

https://github.com/Oxstudent2/logistic_growth/tree/dev2


# 4) (**30 points**) Sometimes we are interested in modelling a process that involves randomness. A good example is Brownian motion. We will explore how to simulate a random process in a way that it is reproducible: - A script for simulating a random_walk is provided in the `question-4-code` folder of this repo. Execute the code to produce the paths of two random walks. What do you observe? (10 points)

what do i observe after running the random walk model

Based on the graph itself – the graph shows the movement of a random particle through a space filled with other moving particles, the line on the graph being the movement pathway of the particle and the colour of the line being the time passed since the particle started moving, the darker the line being the earlier in time since movement commenced and the lighter being the position/movement of the particle later in time. Both the graphs have different scales of the x and y coordinates, indicating each of the particles followed different paths of movement within the space of the graph. The particle in the first graph (left) moved a greater distance on the x and y axis, moving between -5 to 0 on the x axis and -1 to 3 on the y asixs, whereas the second graph (right) only moved between -1 to 3 on the x axis and -2 to 0 on the y axis.
The 2 walks show different patterns of motion and movement – the first one generated (on left) shows a more linear movement and the particle does not reach the area of the graph between the coordinates (-1,0), whereas in the second generated walk, the particles movement is more widespread and covers most of the coordinate regions of the space. The overall movement of the particle on both of the graphs looks relatively random, and the environment is considerably busy as the graph has many loops of the particle moving back on itself, indicating continuous bumping into other particles. In the first graph, there is a short period where the particle moves in a straight direction.

upon running this code again and creating a new model, 2 copletely different graphs are formed with different patterns and directions of movement to those just described, and different movement patterns are generated on each subsequent run.

  # - Investigate the term **random seeds**. What is a random seed and how does it work? (5 points)

A random seed is number/vector used to initiate a psaudorandom number generator. a pseudo random number generator is an algorithm that generates sequences of numbers that are approximately random, and the sequence of numbers is determined by the seed, therefore if the same seed is used for the pseuado number generator instead of a random seed, the same sequence of numbers will be produced. 

In R studio a setting a seed creates a pseudo random number generator, and setting a random seed can be used to generate random numbers. The function itself in R studio is an ‘integer containing vector’, and contains a random number generator and can be used for generating random numbers to create different scenarios in the model or data being observed and tested. This can be used to introduce randomness and therefore reduce the reproducibility of the results.

#  Edit the script to make a reproducible simulation of Brownian motion. Commit the file and push it to your forked `reproducible-research_homework` repo. (10 points)

 script was appropriately eidted and meged to the repo, changes can be seen in files under question 4 code
https://github.com/Oxstudent2/reproducible-research_homework/tree/main/question-4-code

#  Go to your commit history and click on the latest commit. Show the edit you made to the code in the comparison view (add this image to the README.md of the fork). (5 points)
 ![image](https://github.com/Oxstudent2/reproducible-research_homework/assets/150162434/eb8049d5-99d6-458c-8b03-05c84a1ef43f)

 
# Import the data for double-stranded DNA (dsDNA) viruses taken from the Supplementary Materials of the original paper into Posit Cloud (the csv file is in the `question-5-data` folder). How many rows and columns does the table have? (3 points)

33 rows, 9 columns

# What transformation can you use to fit a linear model to the data? Apply the transformation. (3 points)

use a log transfomation on both aspects of the data, so

```
lm(log(virion_volume_nm_nm_nm) ~ log(genome_length_kb), virusdata1)
```

# Find the exponent and scaling factor of the allometric law for dsDNA viruses and write the p-values from the model you obtained, are they statistically significant? Compare the values you found to those shown in Table 2 of the paper, did you find the same values? (10 points)

The exponent (alpha) in an allometric model shows how the parameter Y differs regarding parameter X, and the change seen across x and y – hence it is represented by the slope. The slope of the dsDNA can be worked out form the log linear model I created (I am using the log transformed data as this is what I used to make it fit the linear model):
```
log_virusdata2 <- lm(log(virion_volume_nm_nm_nm) ~ log(genome_length_kb), virusdata1)
summary(log_virusdata2)
```
A summary of this model gives an output of:
```
                     Estimate Std. Error t value Pr(>|t|)    
(Intercept)             7.0748     0.7693   9.196 2.28e-10 ***
log(genome_length_kb)   1.5152     0.1725   8.784 6.44e-10 ***

```
The slope is given in the summary of the log virus data as 1.52, with a p value of 6.44e-10, which much lower than the 0.005 significance value, therefore this result for the slope and therefore exponent is significant. How does this value differ from that in the table in the paper by Cui et al? their value was aslo 1.52.
Regarding the scaling factor beta, I can find it mathematically by substituting in the intercept gained (7.0738) into a linear model with the gradient, and using this to fully determine the scaling factor
```
V = BL^a
```
 when inputting gradinet and the first peice of known data, V = 12.68, L = 4.55
I can substitute V, viron volume and L, genome length into this equation
```
12.68 = X4.55^1.52
```
Now i need to rearrange this equation to solve for X
```
12.68/4.55^152
```
Gives the result 1.267

The exponent I calculated was the same as that found by Cui et al, and was within a 95% signifcance level, therefre this model was significant, however The scaling factor value I calsulated was different, signifcantly lower than the 1,182 found in the table

# Write the code to reproduce the figure shown below. (10 points)

code to reproduce figure:
```
install.packages('ggplot2')
 install.packages('janitor')
library(ggplot2)
library janitor

virusdata <- read.csv('Cui_etal2014.csv')

virusdata1 <- clean_names(virusdata)

log_virusdata <- lm(genome_length_kb ~ log(virion_volume_nm_nm_nm), virusdata1)

ggplot(logVirusdata, aes(x = log.virusdata1.genome_length_kb., y = log.virusdata1.virion_volume_nm_nm_nm.))+

labs(x = 'log[Genome length(kb)]', y = 'log[Viron volume (nm3)]')+

geom_point()+

geom_smooth(method = 'lm')+

theme_bw()
```
# What is the estimated volume of a 300 kb dsDNA virus? (4 points)

Using the values for scaling factor and exponent from Cui et al:

```
1182*300^1.52
```
 =  6884015nm3 in volume

# Bonus (10 points) Explain the difference between reproducibility and replicability in scientific research. How can git and GitHub be used to enhance the reproducibility and replicability of your work? what limitations do they have? (e.g. check the platform protocols.io).

Reproducibility and replicability seem to be the same thing on the surface however they differ slightly. Generally, reproducibility refers to the replication of specific results regarding a hypothesis, not necessarily the entire scientific process and study that led to the discovery of that result. It requires independent study and investigators to reach the same conclusion based on research. 

Replicability on the other hand refers to the ability of a specific scientific trial/method used to obtain a specific result to build confidence in said scientific method. It involves the provision of information to carry out a specific scientific study/method, usually within a paper, which can be used to gain reproducible results, as similar results are expected after replicating an experiment. 

Reproducibility and replicability are interlinked and both work towards similar goals, that being consistency in results and conclusions from independent parties but also consistency of methods to yield similar results. They are important factors of research to ensure results are valid and not being manipulated by bad actors, and hence scientific discovery and progress can be made. Git and git hub would be useful in enhancing replicability of results, especially in analysis and code, as it’s a useful platform for sharing such data. It allows enhanced reproducibility as people can find different data sets or code on the platform and use this to reach the same or different conclusions as the authors of the papers. 

Additionally, as a free platform its accessible to labs of all levels and budgets. Furthermore, the tracking of changes that can be seen on a git hub means the entire process of research is saved and can be used trailed or checked by other individuals. It prevents authors hiding changes they may have made to their method or data to obtain preferable, false results, and therefore ensures honesty in the research report, as the changes cannot be hidden. They allow replicability for similar reasons they allow reproducibility, as exact methods and changes can be tracked using the git and git hub, and the open access of version control allows compatibility with different systems and research. 

It creates a more collaborative and open source of information, and this open nature allows more people to access and check the reproducibility and validness of specific methods used to obtain a hypothesis and the validity of the overall conclusion. 

limitations of git include a lack of frame works and help for data publishing regarding the data collected, and centralized data storage systems can be lost due to faults if the system goes offline for a period.
The public nature of github means data can be shared but also potentialy stolen and coopted without cretid, which can harm scientists work and lower their standing/reputation. Furthermore, if you have an extremely large dataset it may not be shared on github, as it has a size limit of 50mb for files uploaded to rpeositories. therefore, larger datasets unfortunately cannot be shared.

However, the ease of access to data and the tracking of changes make it extremely useful to tracking changes and ensuring maximum reproducibility and replicability, and in cases where the data is les sthan 50mb, is extremely helpful.

## Instructions

The homework for this Computer skills practical is divided into 5 questions for a total of 100 points (plus an optional bonus question worth 10 extra points). First, fork this repo and make sure your fork is made **Public** for marking. Answers should be added to the # INSERT ANSWERS HERE # section above in the **README.md** file of your forked repository.

Questions 1, 2 and 3 should be answered in the **README.md** file of the `logistic_growth` repo that you forked during the practical. To answer those questions here, simply include a link to your logistic_growth repo.

**Submission**: Please submit a single **PDF** file with your candidate number (and no other identifying information), and a link to your fork of the `reproducible-research_homework` repo with the completed answers. All answers should be on the `main` branch.

## Assignment questions 

1) (**10 points**) Annotate the **README.md** file in your `logistic_growth` repo with more detailed information about the analysis. Add a section on the results and include the estimates for $N_0$, $r$ and $K$ (mention which *.csv file you used).
   
2) (**10 points**) Use your estimates of $N_0$ and $r$ to calculate the population size at $t$ = 4980 min, assuming that the population grows exponentially. How does it compare to the population size predicted under logistic growth? 

3) (**20 points**) Add an R script to your repository that makes a graph comparing the exponential and logistic growth curves (using the same parameter estimates you found). Upload this graph to your repo and include it in the **README.md** file so it can be viewed in the repo homepage.
   
4) (**30 points**) Sometimes we are interested in modelling a process that involves randomness. A good example is Brownian motion. We will explore how to simulate a random process in a way that it is reproducible:

   - A script for simulating a random_walk is provided in the `question-4-code` folder of this repo. Execute the code to produce the paths of two random walks. What do you observe? (10 points)
   
   - Investigate the term **random seeds**. What is a random seed and how does it work? (5 points)
   - Edit the script to make a reproducible simulation of Brownian motion. Commit the file and push it to your forked `reproducible-research_homework` repo. (10 points)
   - Go to your commit history and click on the latest commit. Show the edit you made to the code in the comparison view (add this image to the **README.md** of the fork). (5 points)


5) (**30 points**) In 2014, Cui, Schlub and Holmes published an article in the *Journal of Virology* (doi: https://doi.org/10.1128/jvi.00362-14) showing that the size of viral particles, more specifically their volume, could be predicted from their genome size (length). They found that this relationship can be modelled using an allometric equation of the form **$`V = \beta L^{\alpha}`$**, where $`V`$ is the virion volume in nm<sup>3</sup> and $`L`$ is the genome length in nucleotides.

   - Import the data for double-stranded DNA (dsDNA) viruses taken from the Supplementary Materials of the original paper into Posit Cloud (the csv file is in the `question-5-data` folder). How many rows and columns does the table have? (3 points)
   - What transformation can you use to fit a linear model to the data? Apply the transformation. (3 points)
   - Find the exponent ($\alpha$) and scaling factor ($\beta$) of the allometric law for dsDNA viruses and write the p-values from the model you obtained, are they statistically significant? Compare the values you found to those shown in **Table 2** of the paper, did you find the same values? (10 points)
   - Write the code to reproduce the figure shown below. (10 points)

  <p align="center">
     <img src="https://github.com/josegabrielnb/reproducible-research_homework/blob/main/question-5-data/allometric_scaling.png" width="600" height="500">
  </p>

  - What is the estimated volume of a 300 kb dsDNA virus? (4 points)

**Bonus** (**10 points**) Explain the difference between reproducibility and replicability in scientific research. How can git and GitHub be used to enhance the reproducibility and replicability of your work? what limitations do they have? (e.g. check the platform [protocols.io](https://www.protocols.io/)).
