import pandas as pd

temp_data=pd.read_csv(r"C:\Users\rithw\OneDrive\Desktop\Tranzmeo machine test\temperature (1) (1) (1) (1).csv")
print(temp_data.to_string())
#here, we are using 'read_csv' function to load data from CSV file to the pandas dataframe, and saving it to 'temp-data'
#inside 'read_csv' function, we input the file path
#print the dataframe using 'to_string' function, so we can see and inspect full dataframe

#Let us make a copy of the original temperature dataset, for performing operations in this task
temp_data_2005_2014=temp_data.copy()
print(temp_data_2005_2014.to_string())

#Let us filter data between 2005 and 2014
temp_data_2005_2014['Date']=pd.to_datetime(temp_data_2005_2014['Date']) #Here, we are changing the date column of 'temp_data_2005_2014' to datetime format, so that we can easily filter years between 2005 to 2014
temp_data_2005_2014=temp_data_2005_2014[(temp_data_2005_2014['Date'].dt.year >= 2005) & (temp_data_2005_2014['Date'].dt.year <= 2014)] 
#'temp_data_2005_2014['Date'].dt.year' extracts just the year part of each date
#The condition '(temp_data_2005_2014['Date'].dt.year >= 2005) & (temp_data_2005_2014['Date'].dt.year <= 2014)' checks if year is greater than or equal to 2005 and lesser than or equal to 2014
#Part 'temp_data_2005_2014[(temp_data_2005_2014['Date'].dt.year >= 2005) & (temp_data_2005_2014['Date'].dt.year <= 2014)]' filters out the years that comes within this range

#Let us remove leap days (February 29th)
temp_data_2005_2014=temp_data_2005_2014[~((temp_data_2005_2014['Date'].dt.month==2) & (temp_data_2005_2014['Date'].dt.day==29))]
#Part 'temp_data_2005_2014['Date'].dt.month==2' checks if month is 2nd month (i.e., February) from 'Date' column of 'temp_data'
#Part 'temp_data_2005_2014['Date'].dt.day==29' checks if the day in 'Date' column is 29
#So, the condition 'temp_data_2005_2014['Date'].dt.month==2) & (temp_data_2005_2014['Date'].dt.day==29' will be true only for February 29th dates
#Now, we use '~' operator inverts the above mentioned condition , so all dates will be true except for 'February 29'.
#The code 'temp_data_2005_2014[~((temp_data['Date'].dt.month==2) & (temp_data['Date'].dt.day==29))]' will keep all rows except where date is February 29, thus removing leap days

#Let us extract month and date from 'Date' column in 'MM-DD' format, and add it to a new column 'month-day'. This is done, as we have to consider temperatures of each day
temp_data_2005_2014['month-day']=temp_data_2005_2014['Date'].dt.strftime('%m-%d')
#Here, 'temp_data_2005_2014['Date'].dt.strftime('%m-%d')' extracts the month and date from 'Date' column of 'temp_data' in 'MM-DD' format
#'strftime(%m-%d)' function used to format dates in a specific way , here used to showcase month and day only
#'%m' extracts month in two-digit format (e.g., '01' for January, '02' for February, etc.)
#'%d' also extracts day in two-digit format (e.g., '01' for 1st day, etc)
#'-' leaves a hyphen between month and day
print(temp_data_2005_2014.to_string())

#Let us find out record high temperature and record low temperature by day of the year from 2005 to 2014
record_high_temp=temp_data_2005_2014[temp_data_2005_2014['Element']=='TMAX'].groupby('month-day')['Data_Value'].max()
#'temp_data_2005_2014[temp_data['Element']=='TMAX']' filters and keeps only rows where temp is maximum or 'TMAX'
#Then groupby() fnction is used to group the maximum temperatures by month-day, and also return the temperature value
#'max()' is used to find the highest temperature for each calendar day
record_low_temp=temp_data_2005_2014[temp_data_2005_2014['Element']=='TMIN'].groupby('month-day')['Data_Value'].min()
#'temp_data[temp_data_2005_2014['Element']=='TMIN']' filters and keeps only rows where temp is minimum or 'TMIN'
#Then groupby() fnction is used to group the minimum temperatures by month-day, and also return the temperature value
#'min()' is used to find the lowest temperature value for each calendar day
print(record_high_temp)
print(record_low_temp)

Let us plot line graph of record high and low temperatures by day of year from 2005 to 2014
For plotting , we can use 'Matplotlib' library of python. It is widely used for creating visualizations for data analysis.
import matplotlib.pyplot as plt #importing matplotlib library

plt.figure(figsize=(6,4)) #setting figure size, here 6 inch width and 4 inch height
plt.plot(record_high_temp.index, record_high_temp.values, color='red', label='Record high temp') #plt.plot() here creates a line graph for record high temperatures. 'record_high_temp.index' used for x-axis values i.e., days of years from 2005 to 2014. 'record_high_temp.values' used for y-axis values i.e., record high temperatures for corresponding days. 'color' gives required color to th line graph 
plt.plot(record_low_temp.index, record_low_temp.values, color='blue', label='Record low temp') #plt.plot() here creates a line graph for record low temperatures. 'record_low_temp.index' used for x-axis values i.e., days of years from 2005 to 2014. 'record_low_temp.values' used for y-axis values i.e., record low temperatures for corresponding days. 'color' gives required color to th line graph 
plt.fill_between(record_high_temp.index, record_high_temp.values, record_low_temp.values, color='gray') #'plt.fill_between' here used to fill desired color in between record high and low temperatures 
plt.xlabel('Day of the year') #label for x-axis
plt.ylabel('Temperature in C') #label for y-axis
plt.title('Record high and low temperatures by day of year from 2005 to 2014') #title for the line graph
plt.show() #dispay the figure

So, for the above completed task, we executed following operations (summary):
1. Filtered out the temperature data for years 2005 to 2014
2. Removed data points of leap days (February 29) 
3. Extracted the month-day format from 'Date' column, since we have to consider temperature of each day
4. Found out the record high and low temperatures of each day from 2005 to 2014, by grouping the data by day of the month, and fetching maximum and minimum out of it.
5. Plotted line graph, in which one line denotes record high temperature and other shows record low temperature of each day from 2005 to 2014

Overlay a scatter of the 2015 data for any points (highs and lows) for which the ten year record (2005-2014) record high or record low was broken in 2015.

#Creating a copy of original temperature dataset for performing operations in this task
temp_2015=temp_data.copy()

#Now, let us filter temperature data for 2015
temp_2015['Date']=pd.to_datetime(temp_2015['Date']) #Converting 'Date' column to datetime format
temp_2015=temp_2015[temp_2015['Date'].dt.year==2015] #Filtering temperature data for only year 2015 
temp_2015=temp_2015[~((temp_2015['Date'].dt.month==2) & (temp_2015['Date'].dt.day==29))] #Removing leap days (February 29)
temp_2015['month-day']=temp_2015['Date'].dt.strftime('%m-%d') #Extracting month and date from 'Date' column and adding it to new column 'month-day'
print(temp_2015.to_string())

#Let us see the highest and lowest temp of 2015
record_high_2015=temp_2015[temp_2015['Element']=='TMAX'].groupby('month-day')['Data_Value'].max()
record_low_2015=temp_2015[temp_2015['Element']=='TMIN'].groupby('month-day')['Data_Value'].min()
print(record_high_2015)
print(record_low_2015)

plt.figure(figsize=(6,4)) #setting figure size, here 6 inch width and 4 inch height
plt.plot(record_high_temp.index,record_high_temp.values,color='red',label='Record high 2005-2014') #plt.plot() here creates a line graph for record high temperatures. 'record_high_temp.index' used for x-axis values i.e., days of years from 2005 to 2014. 'record_high_temp.values' used for y-axis values i.e., record high temperatures for corresponding days. 'color' gives required color to th line graph 
plt.plot(record_low_2015.index,record_low_temp.values,color='blue',label='Record low 2005-2014') #plt.plot() here creates a line graph for record low temperatures. 'record_low_temp.index' used for x-axis values i.e., days of years from 2005 to 2014. 'record_low_temp.values' used for y-axis values i.e., record low temperatures for corresponding days. 'color' gives required color to th line graph 
broken_high_temp=record_high_2015[record_high_2015 > record_high_temp] #'broken_high_temp' stores those high temperatures from 2015 that are greater than those in 2005-2014
broken_low_temp=record_low_2015[record_low_2015 < record_low_temp] #broken_low_temp stores those low temperatures from 2015 that are lower than those in 2005-2014
plt.scatter(broken_high_temp.index, broken_high_temp.values, color='green')
plt.scatter(broken_low_temp.index, broken_low_temp.values, color='orange')
plt.xlabel('Day of the year')
plt.ylabel('Temperature')
plt.title('Record high and low temp with 2015 record breakers')
plt.show()

Temperature summary of 2015
record_high_2015 = temp_2015[temp_2015['Element'] == 'TMAX'].groupby('month-day')['Data_Value'].max()
record_low_2015 = temp_2015[temp_2015['Element'] == 'TMIN'].groupby('month-day')['Data_Value'].min()

plt.figure(figsize=(6, 4))
plt.plot(record_high_2015.index,record_high_2015.values, color='darkred', label='Highs (2015)')
plt.plot(record_low_2015.index,record_low_2015.values, color='darkblue', label='Lows (2015)')

plt.xlabel("Day of the Year")
plt.ylabel("Temperature (tenths of degrees C)")
plt.title("Temperature Summary for 2015")
plt.legend()
plt.show()
