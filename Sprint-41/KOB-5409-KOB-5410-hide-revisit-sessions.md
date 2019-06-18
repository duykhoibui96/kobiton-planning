# __API__

## __Modify__

`GET v1/sessions`

__Concern__

1. Need to discuss with BA if we show Prime session for specific user ?

On function __db.Session.findAndCountAll__

- Add include model __db.AutomatedExecutions__ with field *required = false*

On function __sessions.rows.map__

- Add filter to exclude sessions related to Prime execution like `sessions.rows.filter(excludePrimeFunc).map(...)`

- Function __excludePrimeFunc__ will check the following things

    1. If field `AutomatedExecutions` is empty -> return true, or else -> move to step 2
    2. If `AutomatedExecutions.executedFromPrime = false` -> return false, or else -> return true

`GET v1/sessions/overview`

__Concern__

1. Need to discuss with BA if we include Prime session for specific user ?

On function __db.Session.findAll__, remove `raw: true`, because we don't want sequelize to flattern the data

On function __db.Session.findAll__, include model __db.AutomatedExecutions__ with field *required = false*

On function __sessions.forEach__, we refactor as:
```
sessions.map((session) => session.toJSON()).filter(excludePrimeFunc).forEach(...)
```
- Function __excludePrimeFunc__ will check the following things

    1. If field `AutomatedExecutions` is empty -> return true, or else -> move to step 2
    2. If `AutomatedExecutions.executedFromPrime = false` -> return false, or else -> return true

`GET v1/sessions/:sessionId`

__Concern__

1. Need to discuss with BA if we show Prime session for specific user ?

2. Need to discuss with BA if we show full Prime session (including Appium Log) for specific user ? (if concern 1 is OK to show Prime session for specific user)

3. Need to discuss with BA if we show full Revisit session (including Appium Log) for specific user ?

Add middleware to prevent external user to access session related to Prime execution __(How ???)__

On function `db.Session.findOne`, include __db.AutomatedExecutions__ with field *required = false*

Not including `appiumPreviewUrl` and `appiumPreviewSize` in log if field __db.AutomatedExecutions__ not empty

# __BOOSTER AUTOMATED EXECUTION RUNNER__
Verify that when session is created, will it send the session id to data hub ?

# __BOOSTER DATA HUB__
Verify that when receive endpoint to `start automated executions`, will it receive any field `sessionId` and save it to db ?