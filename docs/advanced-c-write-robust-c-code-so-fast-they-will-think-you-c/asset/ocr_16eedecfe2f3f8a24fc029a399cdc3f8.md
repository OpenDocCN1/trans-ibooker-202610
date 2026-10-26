```
cout << endl;
}

ostream& operator<<(ostream& os, const record &rec){
    os << rec.customerId << "|" << rec.firstName << "|" << rec.lastName <<
    "|"
        << rec.orderId << "|" << rec.productId << "|" << rec.quantity <<
    "|"
        << fixed << setprecision(2) << rec.totalPriceRegional << "|" << rec.
    currency << "|"
        << fixed << setprecision(2) << rec.totalPriceUsd << endl;
    return os;
}
```

12. Write the following code to define the displayRecords() function:

```
void displayRecords(){
    cout << " Displaying records with '|' delimiter" << endl;
    for (auto rec : vecRecord){
        cout << rec;
    }
    cout << endl;
}
```

13. Create a file named ParseFiles.cpp and call the parseConfig(), fillCurrencyMap(), and parseRecordFile() functions:

```
#include <CommonHeader.h>
// Global variables ...
string configFile = "./parse.conf";
string recordFile;
string currencyFile;
map<string, float> currencyMap;
vector<record>      vecRecord;
int main(){
    // Read Config file to set global configuration variables.
    if (!parseConfig()){
        cerr << "Error parsing Config File " << endl;
        return false;
    }
    // Read Currency file and fill map
    if (!fillCurrencyMap()){
        cerr << "Error setting CurrencyConversion Map " << endl;
        return false;
```