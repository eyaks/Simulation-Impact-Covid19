# Simulation-Impact-Covid19
#PPF1 function
#'initial' is the initial skill of the student
#center π=0
#π+ w/2=0+ 153/2=  76.5
#π- w/2=0- 153/2= -76.5
#Minimum Height (Hmin)= 26
#slope r = 0.15
def PPF1(initial):
    if initial > 76.5:
        return 0
    elif initial < -76.5:
        return 0
    else:
        return 31.75 + 0.15 *(initial+76.5)

import seaborn as sns
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
#to get the same results each time we run the code
np.random.seed(1572)
#generate a random normal distribution with mean 0 and standard devaition 20 we chose 5375 observations because it's the number of observations in the PISA data
data = np.random.normal(0, 14, 5780)
#convert the values into a dataframe
df_simulated=pd.DataFrame(data)
#rename the column initial skill
df_simulated.columns=['initial_skill']
#draw histogram of initial distribution
sns.histplot(data=df_simulated, x="initial_skill", stat='density', color='red')

#draw histogram of initial distribution
sns.histplot(data=df_simulated, x="initial_skill", stat='density', color='red')

#apply the PPF1 on that initial distr to get learning at end of Grade 1
df_simulated['end_of_G1'] = df_simulated['initial_skill'].apply( PPF1) + df_simulated['initial_skill']

#draw histogram of end of G1
sns.histplot(data=df_simulated, x="end_of_G1",stat='density',color="blue")


df_simulated_order=df_simulated
df_simulated_order.describe()

#G1 learning becomes the initial distr for G2
#apply PPF2 on G1 to get learning at end of grade 2
df_simulated_order['end_of_G2'] = df_simulated_order['end_of_G1'].apply(PPF2)+ df_simulated_order['end_of_G1'] 
#draw hist of end of G2
sns.histplot(data=df_simulated_order, x='end_of_G2',stat='density',color="green")


#93% of distr
df_simulated_order=df_simulated_order.sort_values(by=['end_of_G10'], ascending=False)
df_simulated_portion=df_simulated_order.iloc[:5376]
df_simulated_portion.describe()
#histogram of intial and grade 10 after applying coverage rate 93%
plt.figure(figsize=(20,10))
sns.histplot(data=df_simulated_order, x="initial_skill",stat='density',color="orange")
plt.xticks([-50,-25,	0,	25,	50,	75,	100,	125	,150,	175,	200	,225,	250,	275	,300,	325,	350	,375,	400,	425,	450,	475,	500,	525,	550,	575])
sns.histplot(data=df_simulated_portion, x="end_of_G10",stat='density',color="orange")


#shock 1
#copy skills of grades 1 and 2 in a new df 
df_shock1=df_simulated_order[['initial_skill','end_of_G1','end_of_G2']]
#1/3 of learning in grade 3 is removed (3 months closure)
df_shock1['end_of_G3'] = (df_shock1['end_of_G2'].apply( PPF3)*2/3)+df_shock1['end_of_G2']
#draw hist of end of G3
sns.histplot(data=df_shock1, x="end_of_G3",stat='density',color="yellow")
display(df_simulated_order.describe())
display(df_shock1.describe())

#comparison of grade 3 skills withoutshock (green) and with shock(red)
sns.histplot(data=df_simulated_order, x="end_of_G3",stat='density',color="green")
sns.histplot(data=df_shock1, x="end_of_G3",stat='density',color="red")

