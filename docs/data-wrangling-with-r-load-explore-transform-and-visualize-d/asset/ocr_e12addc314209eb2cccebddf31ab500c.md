```
```python
import matplotlib.pyplot as plt
plt.xticks(rotation=90)
plt.scatter(ticket_purchase_hist['transaction_date_time'].dt.date, ticket_purchase_hist['purchase_price'])
```
```

<matplotlib.collections.PathCollection at 0x7f9310af05b0>

![](bbox=[0.12, 0.17, 0.45, 0.35])

Figure 9.68: Matplotlib visualization of ticket purchase history data on a daily basis

We have used an xticks rotation argument to make the date values easy to read. We then create a scatterplot with transaction dates and purchase prices as shown here. We can see that there are more tickets sold in the lower price ranges (dots populated densely), but there are also some tickets sold at a high price range. Let us explore and find out how many tickets are sold above the price range of $120. You could explore further by joining this dataset with other tables to understand the sporting event and also seat type to understand what these tickets correspond to.

```
```python
ticket_purchase_hist[ticket_purchase_hist["purchase_price"]>120]
```
```

| | sporting_event_ticket_id | purchased_by_id | transaction_date_time | transferred_from_id | purchase_price |
|---|---|---|---|---|---|
| 4497 | 67075059 | 6294092 | 2022-12-15 04:24:15 | None | 123.81 |
| 4498 | 67075059 | 6928398 | 2022-12-15 04:25:53 | None | 123.81 |
| 4499 | 67075060 | 6294092 | 2022-12-15 04:24:15 | None | 123.81 |
| 4500 | 67075060 | 6928398 | 2022-12-15 04:25:53 | None | 123.81 |
| 4501 | 67075061 | 6928398 | 2022-12-15 04:25:53 | None | 123.81 |
| 4502 | 67075062 | 6928398 | 2022-12-15 04:25:53 | None | 123.81 |
| 4503 | 67075081 | 4360752 | 2022-12-15 04:25:27 | None | 123.81 |
| 4504 | 67075081 | 6294092 | 2022-12-15 04:24:15 | None | 123.81 |
| 4505 | 67075081 | 6928398 | 2022-12-15 04:25:53 | None | 123.81 |

Figure 9.69: Exploring the tickets that are sold at a high price

This is a starting point and Matplotlib still supports a lot of different visualization options. We will explore another Python visualization library called Seaborn, based on Matplotlib. Seaborn provides a