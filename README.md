# prophet_forecasting
#define different persians holidays
#as we persians have different islamic holidays and alot long weekends i distinguished them in to different part each has its own effect on sales
fitr = pd.DataFrame({
  'holiday' :'fitr',
  'ds' : pd.to_datetime(['2020-06-04', '2020-06-05', '2021-05-13', 
                         '2021-05-14','2022-05-03','2022-05-04',
                       '2023-04-22','2023-04-23']),
  'lower_window' : 0,
  'upper_window' : 1
})

ashoora = pd.DataFrame({
  'holiday' :'ashoora',
  'ds' : pd.to_datetime(['2020-09-07', '2020-09-08','2021-08-18',
                         '2021-08-19','2022-08-07','2022-08-08',
                         '2023-07-27','2023-07-28']),
  'lower_window' : 0,
  'upper_window' : 1
})

nowrouz = pd.DataFrame({
  'holiday' :'nowrouz',
  'ds' : pd.to_datetime(['2020-03-20','2020-03-21','2020-03-22',
                          '2020-03-23','2020-03-24','2020-03-25',
                         '2020-03-26','2020-03-27','2020-03-28',
                         '2021-03-21','2021-03-22','2021-03-23',
                         '2021-03-24','2021-03-25','2021-03-26',
                         '2021-03-27','2021-03-28','2021-03-29',
                         '2022-03-21','2022-03-22','2022-03-23',
                         '2022-03-24','2022-03-25','2022-03-26',
                         '2022-03-27','2022-03-28','2022-03-29',
                         '2023-03-21','2023-03-22','2023-03-23',
                         '2023-03-24','2023-03-25','2023-03-26',
                        '2023-03-27','2023-03-28','2023-03-29']),
  'lower_window' : 0,
  'upper_window' : 1
})

long_weekend = pd.DataFrame({
  'holiday' :'long_weekend',
  'ds' : pd.to_datetime([
      '2021-06-04','2021-06-05','2021-06-06',
      '2021-07-29','2021-07-30','2021-10-22',
      '2021-10-23','2021-10-24','2022-03-18',
      '2022-03-19','2022-03-20','2022-06-03',
      '2022-06-03','2022-06-05','2023-06-04',
      '2023-06-05','2023-09-06','2023-09-07',
      '2023-09-08','2023-09-14', '2023-09-15',
      '2023-09-16','2023-09-22','2023-09-23',
      '2023-09-24']),
  'lower_window' : 0,
  'upper_window' : 1
})
#concatanate them to have a  holiday data frame
holidays = pd.concat((fitr , ashoora , nowrouz,long_weekend ))
#prepare my data frame in prophet format
df2 = df2.rename(columns={'date':'ds', 'keybab_pan':'y'})
df2 = df2[['ds','y']]
#I split data to compare the results you coulod dont do that
## Assuming your DataFrame is named df2
# Calculate the index to split the DataFrame
split_index = int(0.8 * len(df2))

# Split the DataFrame
df_sampled = df2.iloc[:split_index, :]
df_remaining = df2.iloc[split_index:, :]

#fit a model
m = Prophet(holidays=holidays)
m.fit(df_sampled)
future = m.make_future_dataframe(periods = 365)
