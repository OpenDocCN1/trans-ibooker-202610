
在本节中，我们探索了用于有意义分析的数据清洗的不同功能。用户可以进一步探索不同的 Pandas 函数，用于去重以及针对其数据源的其他清洗活动。我们现在将探索数据整理管道的数据丰富阶段。

## 数据丰富

我们已经学习了如何从各种来源提取数据，将其放入通用或期望的结构中，并清洗数据问题。然而，数据仍处于各个来源的孤岛状态，将多个来源的数据结合起来以丰富数据的额外信息将很有价值。

让我们考虑一个用例，我们将为特定球队发布足球比赛日程。在此之前，我们必须了解我们的数据集，以便连接相关表。我们将首先打印源数据库中可用的表列表。

```
con=wr.mysql.connect("wranglerbook")
cursor=con.cursor()
cursor.execute("show tables;")
cursor.fetchall()
```

```
(('mlb_data',),
('name_data',),
('nfl_data',),
('nfl_stadium_data',),
('person',),
('player',),
('seat',),
('seat_type',),
('sport_division',),
('sport_league',),
('sport_location',),
('sport_team',),
('sport_type',),
('sporting_event',),
('sporting_event_ticket',),
('ticket_purchase_hist',))
```

图 9.49：打印体育票务数据集源数据库中的所有表

我们有一个表列表，下面的图表将说明我们数据集中不同表之间的关系：

| 表名 | 描述 |
|---|---|
| mlb_data | 包含 MLB 球员的统计数据和详细信息。 |
| name_data | 关于人员/球员姓名的详细信息，如姓氏或名字。 |
| nfl_data | 包含 NFL 球员的统计数据。 |
| nfl_stadium_data | 包含 NFL 体育场馆的详细信息，如其场地表面、是否封闭/开放等。 |
| person | 包含人员详细信息，并链接到票务购买历史表。 |
| player | 包含球员详细信息及其所属的体育队伍。 |
| seat | 包含与体育场馆位置关联的座位数据。 |
| seat_type | 包含体育场馆座位的详细信息、不同分类及其舒适度排名。 |
| sport_division | 包含关于球队参加的体育联赛分区描述，例如 NFL -> AFC South。 |
| sport_league | 包含关于球队参加的体育联赛描述，例如 MLB、NFL 等。 |
| sport_location | 包含体育场馆/体育场地址和座位容量的信息。 |
| sport_team | 包含关于球队的详细信息、其简称、所属联赛和主场体育场。 |
| sport_type | 包含关于不同运动类型的描述——在这里我们将仅有足球和棒球的数据。 |
| sporting_event | 包含比赛队伍的详细信息以及比赛地点 ID、比赛日期等。 |
| sporting_event_ticket | 包含每个体育赛事购买的票务详细信息，以及购买者信息、票价以及座位等附加信息。 |
| ticket_purchase_hist | 将包含票务购买历史的详细信息以及票务的任何转让情况。 |

表 9.1：表名和描述

我们期望在最终发布的赛程中看到参赛队伍、比赛举办的体育场馆、相应体育场馆的城市和州、比赛日期以及体育场馆的座位容量。凭借前述信息，我们将为特定足球球队（在本例中为 New York Giants）准备赛程。

## Pandas 数据转换操作

第一步是从体育赛事表中获取信息。让我们探索体育赛事表中可用的数据：

sporting_event

| id | sport_type_name | home_team_id | away_team_id | location_id | start_date_time | start_date | sold_out |
|---|---|---|---|---|---|---|---|
| 1 | baseball | 33 | 34 | 20 | 2022-04-02 13:00:00 | 2022-04-02 | 0 |
| 2 | baseball | 33 | 35 | 20 | 2022-04-09 16:00:00 | 2022-04-09 | 0 |
| 3 | baseball | 33 | 36 | 20 | 2022-04-16 16:00:00 | 2022-04-16 | 0 |
| 4 | baseball | 33 | 37 | 20 | 2022-04-23 18:00:00 | 2022-04-23 | 0 |
| 5 | baseball | 33 | 38 | 20 | 2022-04-30 14:00:00 | 2022-04-30 | 0 |

图 9.50：探索 sporting_event 表中可用的数据

现在我们有了体育赛事日期和运动类型，我们将对其进行过滤以筛选足球球队。然而，我们仍需要知道参加比赛的队伍名称。现在我们将 sporting_event 表与 sport_team 表连接以获取队伍名称信息。我们将使用 pandas’ merge (https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.merge.html) 命令来连接 DataFrames。我们将使用 left_on 和 right_on 属性来指定在连接子句中使用的键名称：

```
sporting_event_temp1=pd.merge(sporting_event,sport_team,left_on="home_team_id",right_on="id",how='left')
```

sporting_event_temp1

| id_x | sport_type_name_x | home_team_id | away_team_id | location_id | start_date_time | start_date | sold_out | id_y | name | abbreviated |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | baseball | 33 | 34 | 20 | 2022-04-02 13:00:00 | 2022-04-02 | 0 | 33 | San Diego Padres | |
| 2 | baseball | 33 | 35 | 20 | 2022-04-09 16:00:00 | 2022-04-09 | 0 | 33 | San Diego Padres | |

图 9.51：使用 pandas’ merge 函数连接表

现在我们了解到 4 月 2 日和 4 月 9 日安排的比赛的主队是来自 MLB 联盟的 San Diego Padres。细心的读者会注意到 id 列在 sporting_event（左表）DataFrame 中附加了 _x，在 sport_team（右表）DataFrame 中附加了 _y。这是为了避免结果数据集中出现重复的列名。类似的后缀也会附加到其他具有相似名称的列上。我们必须注意这一点，并在结果 DataFrame 中使用此名称来访问这些列。

这很棒，但你能想到前面方法有什么问题吗？如果你再与另一个也具有名为 id 的列的表进行连接会怎样？这将导致连接条件出错。

并且当结果数据集导出供不同用途使用时，很难记住使用了哪个表作为左表或右表。幸运的是，Pandas 还允许我们为相似的列名添加自定义后缀。我们将使用 _set 表示 sporting_event 表，使用 _se 表示 sporting_event_temp1 表，如以下屏幕截图所示：

![](img/ec1933a597ce63afa3bcf7496364f412_332_0.png)

图 9.52：使用自定义后缀连接表

现在让我们同样进行连接以获取客队信息，如下所示。你可以看到我们在连接语句中对不同的键列 home_team_id 和 away_team_id 进行了连接：

![](img/ec1933a597ce63afa3bcf7496364f412_332_1.png)

图 9.53：连接表以获取主客队信息

现在我们有了参赛体育队伍的信息，我们仍需要比赛举办地点的地点信息/体育场信息。我们将通过连接 sport_location 表来获取该信息，以便它具有体育场信息（名称、城市和州）以及座位容量，如以下屏幕截图所示：

```
sporting_event_temp1=pd.merge(sporting_event,sport_team,left_on="home_team_id",right_on="id",how='left',suffixes=('_set',
sporting_event_temp1=pd.merge(sporting_event_temp1,sport_team,left_on="away_team_id",right_on="id",how='left',suffixes='_
sporting_event_temp1=pd.merge(sporting_event_temp1,sport_location,left_on="location_id",right_on="id",how='left',suffixes=
sporting_event_temp1
```

| _at | sport_type_name | sport_league_short_name_at | sport_division_short_name_at | id_loc | name | city | seating_capacity | levels | sections |
|---|---|---|---|---|---|---|---|---|---|
| 26 | baseball | MLB | AL East | 20 | Petco Park | San Diego California | 40162 | 2 | 40 |
| 27 | baseball | MLB | AL East | 20 | Petco Park | San Diego California | 40162 | 2 | 40 |

图 9.54：连接 sport_location 表以获取体育赛事的体育场/地点信息

现在我们拥有了所有所需字段，但我们有许多需要过滤掉的额外字段。

我们可以利用 df.loc[:, list of columns] 函数来实现这一点，通过仅选择所需的列：

```
sporting_event_temp1=pd.merge(sporting_event,sport_team,left_on="home_team_id",right_on="id",how='left',\
    suffixes=('_se1', '_ht'))
sporting_event_temp1=pd.merge(sporting_event_temp1,sport_team,left_on="away_team_id",right_on="id",how='left',\
    suffixes=('_se2', '_at'))
sporting_event_temp1=pd.merge(sporting_event_temp1,sport_location,left_on="location_id",right_on="id",how='left',\
    suffixes=('_se3', '_loc'))
sporting_event_schedule=sporting_event_temp1.loc[:,['id_se1','sport_type_name_se1','sport_league_short_name_se2',\
    'start_date_time','name_se2','name_at','sport_division_short_name_se2'\
    'sport_division_short_name_at','name','city','seating_capacity']]
sporting_event_schedule
```

|  | id_se1 | sport_type_name_se1 | sport_league_short_name_se2 | start_date_time | name_se2 | name_at | sport_division_short_name_se2 | sport_d |
|---|---|---|---|---|---|---|---|---|
| 0 | 1 | baseball | MLB | 2022-04-02 13:00:00 | San Diego Padres | Tampa Bay Rays |  | AL East |
| 1 | 2 | baseball | MLB | 2022-04-09 16:00:00 | San Diego Padres | Atlanta Braves |  | AL East |
| 2 | 3 | baseball | MLB | 2022-04-16 16:00:00 | San Diego Padres | New York Yankees |  | AL East |
| 3 | 4 | baseball | MLB | 2022-04-23 |  | Texas |  | AL East |

图 9.55：从 pandas DataFrame 中仅选择所需的列

前面的输出仍然具有冗长的列名，并且没有有意义的业务名称。我们可以使用 rename 函数按如下所示更改名称，以更好地表示数据。
