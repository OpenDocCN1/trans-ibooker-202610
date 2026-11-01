| position | player_number | name | status | stat1 | stat1_val | stat2 | stat2_val | stat3 | stat3_val | stat4 | stat4_val | team |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| OT | 66 | Graf, Kevin | ACT | G | -- | GS | -- |  |  |  |  | IND |
| FS | 32 | Green, T.J. | ACT | TCKL | -- | SCK | -- | FF | -- | INT | -- | IND |
| FS | 27 | Guy, Winston | ACT | TCKL | 6 | SCK | 0 | FF | 0 | INT | -- | IND |
| OT | 73 | Haege, Joe | ACT | G | -- | GS | -- |  |  |  |  | IND |

Figure 9.13: Output file with columns containing an escape character wrapper in double quotes

Even if we fix a single file, there are more files with similar issues and the presence of an escape character will cause data filters on that column to not work correctly. So, based on our initial analysis, it would be appropriate to store the raw files in JSON format for further analysis.

6. Change the source code in the Glue console to store the data in a JSON file format. Clone the current Glue job and change the script to that shown here:

```
import sys
import awswrangler as wr
import time
import csv
#Connect to MySQL using Glue connections
con=wr.mysql.connect("wranglerbook")
schema="dms_sample"
table_list=
["mlb_data","name_data","nfl_data","nfl_stadium_data","person","player","seat","seat_type","sport_division","sport_league","sport_location","sport_team","sport_type","sporting_event","ticket_purchase_hist"]
table_list=["nfl_data"]
for table in table_list:
    #Read data from MySQL tables into Pandas dataframe
    start_time = time.time()
    output_data=wr.mysql.read_sql_query("SELECT * FROM "+schema+"."+ table +";", con=con)
    #Print one row for debugging
    #print(output_data.loc[1,:])
    if len(output_data.axes[0])>0:
        #Write Pandas dataframe into S3
        wr.s3.to_json(
            df=output_data, path="s3://wranglerbook/awswrangler/"+table+"/",
            dataset=True,
            orient='records',
            lines=True,
            mode="overwrite",
            database="mysql_extract",
            table="mysql1_"+table)
        end_time=time.time()
        print(f"Completed the data extraction for table - {table} .Loaded record count - {len(output_data.axes[0])} and took {round(end_time-start_time,2)} seconds")
    else:
        print(f"Data load not performed for table {table} as it is empty.")
print("Completed the data extraction process")
con.close()
```

If you notice the difference, we have only changed the to_csv function into to_json. This illustrates the ease with which we can save files to S3.

Now you can see the data is correctly populated as JSON files and can verify this from S3 Select, as shown here.