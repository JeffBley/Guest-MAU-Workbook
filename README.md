# Guest-MAU-Workbook
A log analytics workbook that will help admins estimate the B2B guest MAU as well as telephony-based MFA usage. This can help predict the cost of moving to MAU-based licensing model.

## Steps to implement
1. Navigate to portal.azure.com and search for and select **Log Analytics Workspaces**.
2. Select the workspace that has your Azure AD sign-in logs.
  - If you don't know which workbook has the Azure AD sign-in logs, go to Azure Active Directory -> Diagnostic Settings to see which workspaces are configured to recieve the sign-in logs. If multiple workspaces appear, you'll have click **edit** and see which one has the box checked for sign-in logs.
![image](https://user-images.githubusercontent.com/49490355/171745434-4b9ee97b-f860-4056-bd11-67fe3fadb5b6.png)


3. Select **Workbooks** and click **New**.
![image](https://user-images.githubusercontent.com/49490355/171745890-426674ea-72f9-466c-a1fd-6d2934dc895f.png)


4. Click the **Advanced Editor** button which looks like "</>".
![image](https://user-images.githubusercontent.com/49490355/171746129-24cf2372-9d12-494e-bf99-b256d93fea8e.png)


5. Delete the existing text and copy/paste the template below. Then click **Apply**.
![image](https://user-images.githubusercontent.com/49490355/171746756-c5e02178-c9e8-49e1-8cf1-991fc8c1d1ac.png)

```
{
  "version": "Notebook/1.0",
  "items": [
    {
      "type": 1,
      "content": {
        "json": "# Guest Monthly Active Users\n---\n\nUse this workbook to help estimate the cost of External Identities in your tenant. [Learn more about pricing](https://azure.microsoft.com/en-us/pricing/details/active-directory/external-identities)\n\n**Note:** The estimate of text messages and phone calls are based on unique authentication requests. Multiple texts or calls in the same sign-in attempt are only counted once in this report. The actual number of texts and calls is likely to be higher than this estimate."
      },
      "name": "text - 2"
    },
    {
      "type": 1,
      "content": {
        "json": "### Filters"
      },
      "name": "text - 1"
    },
    {
      "type": 9,
      "content": {
        "version": "KqlParameterItem/1.0",
        "parameters": [
          {
            "id": "bf9b3709-b9e1-4e9a-a483-862c2b7237cb",
            "version": "KqlParameterItem/1.0",
            "name": "Workbook",
            "type": 5,
            "isRequired": true,
            "value": "/subscriptions/ab48f397-fc82-4634-aa52-62dd91b3ebaa/resourceGroups/Woodgrove-RG/providers/Microsoft.OperationalInsights/workspaces/Woodgrove-LogAnalyiticsWorkspace",
            "typeSettings": {
              "additionalResourceOptions": []
            },
            "timeContext": {
              "durationMs": 86400000
            }
          },
          {
            "id": "5da9c644-d229-47a0-8a12-07856b967944",
            "version": "KqlParameterItem/1.0",
            "name": "Subscription",
            "type": 6,
            "isRequired": true,
            "value": "/subscriptions/ab48f397-fc82-4634-aa52-62dd91b3ebaa",
            "typeSettings": {
              "additionalResourceOptions": [],
              "includeAll": false
            },
            "timeContext": {
              "durationMs": 86400000
            }
          }
        ],
        "style": "pills",
        "queryType": 0,
        "resourceType": "microsoft.operationalinsights/workspaces"
      },
      "name": "parameters - 2"
    },
    {
      "type": 9,
      "content": {
        "version": "KqlParameterItem/1.0",
        "parameters": [
          {
            "id": "4dde6989-e1be-4aa7-8ecd-110c5b71e289",
            "version": "KqlParameterItem/1.0",
            "name": "Time",
            "type": 4,
            "isRequired": true,
            "value": {
              "durationMs": 15721200000,
              "endTime": "2022-06-01T22:13:00.000Z"
            },
            "typeSettings": {
              "selectableValues": [
                {
                  "durationMs": 2592000000
                },
                {
                  "durationMs": 5184000000
                },
                {
                  "durationMs": 7776000000
                }
              ],
              "allowCustom": true
            },
            "timeContext": {
              "durationMs": 86400000
            }
          },
          {
            "id": "5fb67ab7-a69b-4ade-8942-8a2373f85412",
            "version": "KqlParameterItem/1.0",
            "name": "License",
            "type": 2,
            "isRequired": true,
            "isGlobal": true,
            "value": "P1",
            "typeSettings": {
              "additionalResourceOptions": [],
              "showDefault": false
            },
            "jsonData": "[\r\n    { \"value\":\"P1\", \"label\":\"Premium P1\" },\r\n    { \"value\":\"P2\", \"label\":\"Premium P2\" }\r\n]",
            "timeContext": {
              "durationMs": 86400000
            }
          },
          {
            "id": "184fc355-08ba-4296-aaf8-9329096a1816",
            "version": "KqlParameterItem/1.0",
            "name": "AuthMethod",
            "type": 2,
            "multiSelect": true,
            "quote": "'",
            "delimiter": ",",
            "value": [
              "value::all"
            ],
            "typeSettings": {
              "additionalResourceOptions": [
                "value::all"
              ],
              "showDefault": false
            },
            "jsonData": "[\r\n    { \"value\":\"Text message\", \"label\":\"Text Message\" },\r\n    { \"value\":\"Phone call approval (Authentication phone)\", \"label\":\"Phone Call Approval\" }\r\n]",
            "timeContext": {
              "durationMs": 86400000
            },
            "label": "Auth method"
          },
          {
            "id": "2aa7874b-aacc-454d-8e76-d74a35346718",
            "version": "KqlParameterItem/1.0",
            "name": "Username",
            "label": "UserPrincipalName",
            "type": 1,
            "timeContext": {
              "durationMs": 86400000
            },
            "value": ""
          }
        ],
        "style": "pills",
        "queryType": 0,
        "resourceType": "microsoft.operationalinsights/workspaces"
      },
      "name": "parameters - 3"
    },
    {
      "type": 1,
      "content": {
        "json": "For the most accurate report, please use a custom time scope to ensure that all days of a month are counted. Also consider your log retention policy. As logs fall beyond the retention timeframe, they will stop being counted in this report."
      },
      "name": "text - 8"
    },
    {
      "type": 3,
      "content": {
        "version": "KqlItem/1.0",
        "query": "let mau = toscalar(SigninLogs\r\n| where TimeGenerated < startofmonth(now())\r\n| where TimeGenerated >= startofmonth(datetime_add('month',-1,now()))\r\n| where UserType == \"Guest\"\r\n| project UserId\r\n| distinct UserId\r\n| count);\r\nlet phone = toscalar(SigninLogs\r\n| where TimeGenerated < startofmonth(now())\r\n| where TimeGenerated >= startofmonth(datetime_add('month',-1,now()))\r\n| where UserType == \"Guest\"\r\n| where AuthenticationRequirement contains \"multifactor\"\r\n| mv-expand ParsedFields=parse_json(AuthenticationDetails)\r\n    |extend AuthenticationMethod = ParsedFields.authenticationMethod\r\n    |extend AuthMethod = tostring(AuthenticationMethod)\r\n| where AuthMethod in ({AuthMethod}) or '*' in ({AuthMethod})\r\n| distinct CorrelationId\r\n| count);\r\nlet price = iff(\"{License}\" == 'P1', 0.00325, 0.01625);\r\nlet maucost=max_of(0,mau - 50000) * price;\r\nlet phonecost=phone * 0.03;\r\nlet totalcost= phonecost + maucost;\r\n\r\nlet View_1 = view () { print tostring(mau),\"MAU\" };\r\nlet View_2 = view () { print strcat(\"$\",tostring(maucost)),\"MAU cost (USD)\" };\r\nlet View_3 = view () { print tostring(phone),\"SMS/Voice\" };\r\nlet View_4 = view () { print strcat(\"$\",tostring(phonecost)),\"SMS/Voice cost (USD)\" };\r\nlet View_5 = view () { print strcat(\"$\",tostring(totalcost)),\"Total cost (USD)\" };\r\nunion withsource=TableName View_1, View_2, View_3, View_4, View_5\r\n\r\n",
        "size": 3,
        "showAnalytics": true,
        "title": "Estimate of last month's guest activity",
        "showRefreshButton": true,
        "queryType": 0,
        "resourceType": "microsoft.operationalinsights/workspaces",
        "visualization": "tiles",
        "tileSettings": {
          "titleContent": {
            "columnMatch": "print_1"
          },
          "leftContent": {
            "columnMatch": "print_0",
            "formatter": 12,
            "formatOptions": {
              "palette": "blue"
            },
            "numberFormat": {
              "unit": 0,
              "options": {
                "style": "decimal"
              }
            }
          },
          "showBorder": false,
          "sortCriteriaField": "print_1"
        }
      },
      "showPin": true,
      "name": "query - 12"
    },
    {
      "type": 1,
      "content": {
        "json": "You can reduce SMS/Voice costs by enabling the [MFA Registration Campaign](https://docs.microsoft.com/en-us/azure/active-directory/authentication/how-to-mfa-registration-campaign) for guests."
      },
      "name": "text - 4"
    },
    {
      "type": 3,
      "content": {
        "version": "KqlItem/1.0",
        "query": "SigninLogs\r\n| where UserType == \"Guest\"\r\n| extend MonthGenerated = startofmonth(TimeGenerated)\r\n| project MonthGenerated,UserId\r\n| distinct UserId,MonthGenerated\r\n| make-series distinctUserCount=count() default=0 on MonthGenerated from (startofmonth({Time:start})) to (startofmonth(now()) - 1h) step 1d\r\n| mv-expand distinctUserCount to typeof(int), MonthGenerated to typeof(datetime)\r\n| project distinctUserCount, MonthGenerated\r\n| summarize Value=sum(distinctUserCount) by dateStr=format_datetime(startofmonth(MonthGenerated), 'MM/yyyy')\r\n",
        "size": 0,
        "showAnalytics": true,
        "title": "Guest Monthly Active Users trends",
        "timeContextFromParameter": "Time",
        "showRefreshButton": true,
        "queryType": 0,
        "resourceType": "microsoft.operationalinsights/workspaces",
        "visualization": "linechart",
        "chartSettings": {
          "xAxis": "dateStr",
          "group": "Grouping",
          "createOtherGroup": null,
          "showMetrics": false
        }
      },
      "customWidth": "70",
      "showPin": true,
      "name": "query - 6"
    },
    {
      "type": 3,
      "content": {
        "version": "KqlItem/1.0",
        "query": "SigninLogs\r\n| where UserType == \"Guest\"\r\n| extend MonthGenerated = startofmonth(TimeGenerated)\r\n| project MonthGenerated,UserId\r\n| distinct UserId,MonthGenerated\r\n| make-series distinctUserCount=count() default=0 on MonthGenerated from (startofmonth({Time:start})) to (startofmonth(now()) - 1h) step 1d\r\n| mv-expand distinctUserCount to typeof(int), MonthGenerated to typeof(datetime)\r\n| project distinctUserCount, MonthGenerated\r\n| summarize MAU=sum(distinctUserCount) by Month=format_datetime(startofmonth(MonthGenerated), 'MM/yyyy')\r\n",
        "size": 0,
        "timeContextFromParameter": "Time",
        "queryType": 0,
        "resourceType": "microsoft.operationalinsights/workspaces",
        "visualization": "table",
        "gridSettings": {
          "formatters": [
            {
              "columnMatch": "Month",
              "formatter": 1,
              "formatOptions": {
                "customColumnWidthSetting": "50%"
              }
            },
            {
              "columnMatch": "MAU",
              "formatter": 1,
              "formatOptions": {
                "customColumnWidthSetting": "50%"
              }
            }
          ]
        }
      },
      "customWidth": "30",
      "name": "query - 9"
    },
    {
      "type": 3,
      "content": {
        "version": "KqlItem/1.0",
        "query": "SigninLogs\r\n| where UserType == \"Guest\"\r\n| where AuthenticationRequirement contains \"multifactor\"\r\n| mv-expand ParsedFields=parse_json(AuthenticationDetails)\r\n    |extend AuthenticationMethod = ParsedFields.authenticationMethod\r\n    |extend AuthMethod = tostring(AuthenticationMethod)\r\n| where AuthMethod in ({AuthMethod}) or '*' in ({AuthMethod})\r\n| where \"{Username:escape}\" == \"All users\" or UserDisplayName contains \"{Username:escape}\"\r\n| extend MonthGenerated = startofmonth(TimeGenerated)\r\n| project AuthMethod, MonthGenerated, CorrelationId\r\n| distinct AuthMethod, MonthGenerated, CorrelationId\r\n| make-series TelephonyCount=count() default=0 on MonthGenerated from (startofmonth({Time:start})) to (startofmonth(now()) - 1h) step 1d\r\n| mv-expand TelephonyCount to typeof(int), MonthGenerated to typeof(datetime)\r\n| extend Grouping = \"All\" //Only used for connecting the dots in the line graph visualization\r\n| summarize Telephony=sum(TelephonyCount) by dateStr=format_datetime(startofmonth(MonthGenerated), 'MM/yyyy'), Grouping\r\n",
        "size": 0,
        "showAnalytics": true,
        "title": "Guest SMS/Voice usage",
        "timeContextFromParameter": "Time",
        "showRefreshButton": true,
        "queryType": 0,
        "resourceType": "microsoft.operationalinsights/workspaces",
        "visualization": "linechart",
        "chartSettings": {
          "xAxis": "dateStr",
          "group": "Grouping",
          "createOtherGroup": null,
          "showMetrics": false,
          "xSettings": {
            "numberFormatSettings": {
              "unit": 0,
              "options": {
                "style": "decimal",
                "useGrouping": true
              }
            }
          }
        }
      },
      "customWidth": "70",
      "showPin": true,
      "name": "query - 7"
    },
    {
      "type": 3,
      "content": {
        "version": "KqlItem/1.0",
        "query": "SigninLogs\r\n| where UserType == \"Guest\"\r\n| where AuthenticationRequirement contains \"multifactor\"\r\n| mv-expand ParsedFields=parse_json(AuthenticationDetails)\r\n    |extend AuthenticationMethod = ParsedFields.authenticationMethod\r\n    |extend AuthMethod = tostring(AuthenticationMethod)\r\n| where AuthMethod in ({AuthMethod}) or '*' in ({AuthMethod})\r\n| where \"{Username:escape}\" == \"All users\" or UserDisplayName contains \"{Username:escape}\"\r\n| extend MonthGenerated = startofmonth(TimeGenerated)\r\n| project AuthMethod, MonthGenerated, CorrelationId\r\n| distinct AuthMethod, MonthGenerated, CorrelationId\r\n| make-series TelephonyCount=count() default=0 on MonthGenerated from (startofmonth({Time:start})) to (startofmonth(now()) - 1h) step 1d\r\n| mv-expand TelephonyCount to typeof(int), MonthGenerated to typeof(datetime)\r\n| summarize Telephony=sum(TelephonyCount) by Month=format_datetime(startofmonth(MonthGenerated), 'MM/yyyy')",
        "size": 0,
        "timeContextFromParameter": "Time",
        "queryType": 0,
        "resourceType": "microsoft.operationalinsights/workspaces",
        "gridSettings": {
          "formatters": [
            {
              "columnMatch": "Month",
              "formatter": 1,
              "formatOptions": {
                "customColumnWidthSetting": "50%"
              }
            },
            {
              "columnMatch": "TelephonyMFA",
              "formatter": 1,
              "formatOptions": {
                "customColumnWidthSetting": "50%"
              }
            }
          ],
          "labelSettings": [
            {
              "columnId": "Telephony",
              "label": "SMS/Voice"
            }
          ]
        }
      },
      "customWidth": "30",
      "name": "query - 10"
    }
  ],
  "fallbackResourceIds": [
    "/subscriptions/ab48f397-fc82-4634-aa52-62dd91b3ebaa/resourceGroups/Woodgrove-RG/providers/Microsoft.OperationalInsights/workspaces/Woodgrove-LogAnalyiticsWorkspace"
  ],
  "$schema": "https://github.com/Microsoft/Application-Insights-Workbooks/blob/master/schema/workbook.json"
}
```

6. Set the filters to the correct values as needed and specify a custom time range in order to count all the days in a given month.

You're done!
