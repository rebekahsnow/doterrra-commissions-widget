# doterrra-commissions-widget
The Doterra Commissions Widget shows the total amount of money each Wellness Advocate has been paid per month. This information was previously only available within their website's back office portal. Now, users can stay up to date on their commissions. The widgets are available in three sizes, depending on how much information each user prefers. 

Working on this project was an amazing experience. I was able to spearhead it with the help of two back-end developers and our usability research team. I loved the opportunity to contribute my opinions about the design and back-end sorting solutions while also thinking of creative solutions to users' pain points.

ℹ️ The following documentation is intended for internal use only. Some information has been redacted for privacy. See below for examples of the documentation experience.

## Promo Videos 
https://www.youtube.com/watch?v=GRjHj73SBa8
https://www.youtube.com/watch?v=YxJMa38EimQ

## Design / UI
### Final Design Screenshots 
<img width="840" alt="Screenshot 2024-10-01 at 12 14 43 AM" src="https://github.com/user-attachments/assets/26a5e726-8fe9-4370-9445-4a3b93599075">

### Dates on graph 

Feedback showed graph could be confusing so dates and points were added. The dates represent the first and last point of graph. 

ℹ️ Japan and Mexico market prefer different date formats. Those can be found in the translation files. 

![image](https://github.com/user-attachments/assets/6e877e93-d9a1-48e9-a084-77377752efa3)



### Font sizes
Small widget: 10px 

Medium & large widget: 12px

### Calculating Points
The low points on the graph are NOT zero but they are the minimum amount made in the previous 12 months. 


```
static func calculateDataPoints(earnings: [Double]) -> [Double] {
        guard let minval = earnings.min(), let maxval = earnings.max(), minval != maxval else {
            // Handle the case where all values are zero
            if earnings.allSatisfy({ $0 == 0 }) {
                return Array(repeating: 0, count: earnings.count)
            } else {
                return []
            }
        }
        return earnings.map { (earning) in
            let proportion = (earning - minval) / (maxval - minval)
            return proportion
        }
    }
```
### Displaying large values 
Set OV AND PV font to smaller value than in Figma design if:
* OV >= 10,000 OR PV >= 10,000 (SMALL widget) 
* OV >= 1,000,000 OR PV >= 1,000,000 (MED/LG widget)


## API Calls
Postman links containing more details are located on each header. (Link redacted for privacy) 

To get DISTID for following calls, check if user is logged in and get from app. Otherwise, show “not logged in” screen.

### Get Income Value (Earnings History)
There are two types of incomes a WA can make, primary bonus and secondary bonus. These are paid at different frequencies and at different times. 
A function needs to be implemented to find total for each month because there is currently no API hit for that. For iOS project this functionality is found in the WidgetDataMapper.swift file -addWeeklyIncomeIntoMonths


### Primary Bonus
Primary bonus is paid monthly between 15th and 20th. 

periodcount: 12 

periodtype: PB

### Secondary Bonus
Paid weekly. Because of this the first week of current month will always show as $0. 

periodcount: 52

periodtype: SB

## Get PV and OV Values (Earnings Summary) 
For each month there is an associated PV (personal volume) and OV (organization volume) for each user.

You will need to call .getSummary four separate times to get current month and 3 previous months. 

ℹ️ Example of date format (YYYYMM) to enter for January 2024: 202401. 
