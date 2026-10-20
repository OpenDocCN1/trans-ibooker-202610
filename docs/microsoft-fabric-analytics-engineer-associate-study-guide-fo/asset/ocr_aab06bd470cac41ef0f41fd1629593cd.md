# Get the latest data in real time with DirectQuery (Premium only)

You can check this box to add a DirectQuery partition to fetch the latest changes beyond the refresh period.

# Only refresh complete days

You can check this box to ensure that only full days are refreshed. This prevents partial updates, which is important if your metrics rely on complete daily data.

# Detect data changes

You can check this box to make Power BI refresh only periods where changes have happened, based on a separate Last modified column. This reduces refresh time even more if the data is stable.