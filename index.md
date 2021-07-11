## About

This page holds information on how to incorporate Python into understanding and evaluating proxy records (in this case G. ruber δ18O data) in the Eastern Equatorial Pacific. All of the data can be found on the Wiki page of this repository. The code below is an portion of the work I did for Celeste Pallone and Jerry McManus from my internship with them at Lamont-Doherty Earth Observatory at Columbia University. If you have any questions or comments feel free to message me at @BrynGlacier on Twitter. 

### Obersving How δ18O Values Change With Latitude
Below I am investigating to see the relationship between all δ18O values during each of their respectve studies and the latitude at which the core holding the δ18O values is located. 
#### Boxplot Figure with swarmplot


```markdown
#Visualization Set Up
sns.set(rc={'figure.figsize':(15,8)})
sns.set_context("paper")
sns.set_theme(style="ticks")

#Plot
sns.boxplot(x=df.Latitude, y=df.d18O, hue=df.Core,linewidth=2.5,palette = 'coolwarm', dodge=False)
sns.swarmplot(x=df.Latitude, y=df.d18O, color=".25")
plt.legend(loc='lower center',frameon=False, fontsize=15,ncol=len(df.columns),bbox_to_anchor=(.5, -0.3))
plt.gca().invert_yaxis()

# Labels

sns.despine(top= True, right= True)
plt.xlabel('Latitude', fontsize=15, fontweight='bold') 
plt.ylabel('δ18O [‰]', fontsize=15, fontweight='bold')
plt.title('G. ruber δ18O Values Depending on Latitude in the EEP',fontsize=20, fontweight='bold')
```
![Latitude_Image_01](https://user-images.githubusercontent.com/71152705/125203094-eaceb180-e244-11eb-9ce0-ba0c5b9b4fa3.png)

#### Boxplot Figure without swarmplot
```markdown
#Visualization Set Up
sns.set(rc={'figure.figsize':(15,8)})
sns.set_context("paper")
sns.set_theme(style="ticks")

#Plot
sns.boxplot(x=df.Latitude, y=df.d18O, hue=df.Core,linewidth=2.5,palette = 'coolwarm', dodge=False)
plt.legend(loc='lower center',frameon=False, fontsize=15,ncol=len(df.columns),bbox_to_anchor=(.5, -0.3))
plt.gca().invert_yaxis()

# Labels

sns.despine(top= True, right= True)
plt.xlabel('Latitude', fontsize=15, fontweight='bold') 
plt.ylabel('δ18O [‰]', fontsize=15, fontweight='bold')
plt.title('G. ruber δ18O Values Depending on Latitude in the EEP',fontsize=20, fontweight='bold')
```
![Latitude_Image_02](https://user-images.githubusercontent.com/71152705/125203147-4dc04880-e245-11eb-9851-b256ceee9a56.png)

#### Age Models 
When making my age models, I attempted to connect the δ18O values with the depth at which those δ18O values were collected. Since there is not a standard space between the depths at which the forams were collected, I made several functions that calculate the age given the difference in depth and δ18O value. Below is all of the code I used to make the age model of Rippert et al. 2017 data. While it is not perfect towards the end, it is mostly incredibly accurate. 

{::options parse_block_html="true" /}

<details>
  <summary markdown="span">Code for Age Model</summary>
  
  ```python
	# Depth - m
	# Age - ky BP
	# d18O - [per mil PDB] G.hexagonus

	df = pd.read_excel('odp1240age.xlsx') #download data 
	df.head()

	#find difference in depths for each sample collected
	d = df.CoreDepth
	XValNew = []
	for i in range(1, len(d)):
		XValNew.append(d[i]-d[i-1])

	#find difference in age for each sample collected
	y = df.Age
	YValNew = []
	for i in range(1, len(y)):
		YValNew.append(y[i]-y[i-1])

	#calculate slope
	Slope = np.divide(YValNew,XValNew)

	#finding y-intercept 
	B=[]
	for i in range(len(Slope)):
		B.append(y[i] - (Slope[i] * d[i]))

	n = [] #age this program is finding 

	i = np.arange(0.01, 30, 0.01) #depth of the Rippert et al. 2017 sample
		
		def n(i):
    if (0.01<=i<=0.25): 
        return Slope[0]*i + B[0]
    elif (0.25<i<=0.77):
        return Slope[1]*i + B[1]
    elif (0.77<i<=1.17):
        return Slope[2]*i + B[2]
    elif (1.17<i<=1.38):
        return Slope[3]*i + B[3]
    elif (1.38<i<=1.51):
        return Slope[4]*i + B[4]
    elif (1.51<i<=1.75):
        return Slope[5]*i + B[5]
    elif (1.75<i<=2.23):
        return Slope[6]*i + B[6]
    elif(2.23<i<=2.31):
        return Slope[7]*i + B[7]
    elif(2.31<i<=2.63):
        return Slope[8]*i + B[8]
    elif(2.63<i<=2.91):
        return Slope[9]*i + B[9]
    elif(2.91<i<=3.18):
        return Slope[10]*i + B[10]
    elif(3.18<i<=3.5):
        return Slope[11]*i + B[11]
    elif(3.5<i<=3.62):
        return Slope[12]*i + B[12]
    elif(3.62<i<=4.42):
        return Slope[13]*i + B[13]
    elif(4.42<i<=4.86):
        return Slope[14]*i + B[14]
    elif(4.86<i<=5.16):
        return Slope[15]*i + B[15]
    elif(5.16<i<=5.65):
        return Slope[16]*i + B[16]
    elif(5.65<i<=6.43):
        return Slope[17]*i + B[17] 
    elif(6.43<i<=7.59):
        return Slope[18]*i + B[18]
    elif(7.59<i<=9.81):
        return Slope[19]*i + B[19]
    elif(9.81<i<=10.21):
        return Slope[20]*i + B[20]
    elif(10.21<i<=10.91):
        return Slope[21]*i + B[21]
    elif(10.91<i<=11.39):
        return Slope[22]*i + B[22]
    elif(11.39<i<=12.71):
        return Slope[23]*i + B[23]
    elif(12.71<i<=13.74):
        return Slope[24]*i + B[24]
    elif(13.74<i<=14.09):
        return Slope[25]*i + B[25] 
    elif(14.09<i<=14.89):
        return Slope[26]*i + B[26]
    elif(14.89<i<=17.35):
        return Slope[27]*i + B[27]
    elif(17.35<i<=18.53):
        return Slope[28]*i + B[28]
    elif(18.53<i<=18.94):
        return Slope[29]*i + B[29]
    elif(18.94<i<=20.63):
        return Slope[30]*i + B[30]
    elif(20.63<i<=22.02):
        return Slope[31]*i + B[31]
    elif(22.02<i<=22.86):
        return Slope[32]*i + B[32]
    elif(22.86<i<=24.44):
        return Slope[33]*i + B[33]
    elif(24.44<i<=24.92):
        return Slope[34]*i + B[34]
    elif(24.92<i<=27.08):
        return Slope[35]*i + B[35]
    elif(27.08<i<=27.75):
        return Slope[36]*i + B[36]
    elif(27.75<i<=28.64):
        return Slope[37]*i + B[37]
    elif(28.64<i<=29.39):
        return Slope[38]*i + B[38]
	
	#Plot the function above
	#Visualization Set Up
sns.set(rc={'figure.figsize':(11.7,8.27)})
sns.set_context("paper")
sns.set_theme(style="ticks")
# Declaring the points for first line plot
X1 = df.CoreDepth
Y1 = df.Age #real age
#Plot
plt.plot(X1, Y1, label = "Age (Real)", linewidth=10) 
plt.plot(i, list(map(n, i)), linewidth=3, label = "Age (Calculated)") 
plt.xticks(fontsize=15) 
plt.yticks(fontsize=15) 
#Label
sns.despine(top= True, right= True)
plt.xlabel('Depth (m)', fontsize=15, fontweight='bold') 
plt.ylabel('Age (kyr BP)', fontsize=15, fontweight='bold')
plt.legend(loc='upper left', frameon=False, fontsize=15)
plt.title(' Age Model Comparison (Real Age vs. Calculated Age Model)', fontsize=20, fontweight='bold') 
plt.show()
  
  ```
	![Age_Model](https://user-images.githubusercontent.com/71152705/125204123-9f6ad200-e249-11eb-913b-b0e0666fe623.png)

</details>
<br/>

{::options parse_block_html="false" /}
