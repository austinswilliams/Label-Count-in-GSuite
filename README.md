# Label-Count-in-GSuite
Using GAM to pull labels from GSuite account and Powershell to parse data we can see how many labels each user has

//Step 1 - Use GAM to pull labels for each user.  Use optional [showcounts] if you want to see how many emails are in each label (not recommended)

gam all users show labels > labelsbyuser.csv

//Step 2 - Use powershell to parse data from labelsbyuser.csv to create report of how many labels each user has

$list = Import-Csv labelsbyuser.csv
 $(foreach ($entry in $list)
{
    "$($entry.user),$(select-string -Path $($entry.csv) -Pattern "id" | Measure-Object | Foreach count)"
  }) | Out-File labelsbyuserReport.csv
  
  //Note - id is used in the labelsbyuser.csv gam report to identify each unique label.  The powershell script will sift through each instance of "id" and report how many times it is found for each user, thus giving us the report of how many unique labels each user owns.
