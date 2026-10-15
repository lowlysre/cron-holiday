# 🎸 cron-holiday

This GitHub Action prevents scheduled workflows from running on specific dates (like company holidays or national breaks). It checks if the current day matches a list of YYYY-MM-DD holidays and outputs `on_holiday=true` so you can conditionally skip jobs — or stop the run altogether.

> “This is the dawning of the rest of our lives... on holiday.”
>
> -- Green Day, probably talking about CI/CD

## Tutorial: Getting Started

This tutorial will help you set up `cron-holiday` in your GitHub Actions workflow to skip jobs on holidays.

1. **Add the Action to your workflow:**

   ```yaml
   jobs:
     check-holiday:
       uses: lowlysre/cron-holiday@v1.0.0
       with:
         holidays: "2026-01-01,2026-07-03,2026-12-25"
         timezone: "America/New_York"
         fail-on-holiday: false
       id: holiday

     your-job:
       if: ${{ steps.holiday.outputs.on_holiday != 'true' }}
       runs-on: ubuntu-latest
       steps:
         - run: echo "Doing useful work... because it's not a holiday!"
   ```

2. **Commit and push your workflow file.**
   Your workflow will now skip jobs on the specified holidays.

---

## How-to Guide: Common Tasks

### Skip a Job on Holidays

- Use the output `on_holiday` to conditionally run jobs:

  ```yaml
  if: ${{ steps.holiday.outputs.on_holiday != 'true' }}
  ```

### Fail the Workflow on Holidays

- Set `fail-on-holiday: true` to make the workflow fail if today is a holiday:

  ```yaml
  with:
    fail-on-holiday: true
  ```

### Use a Custom Timezone

- Specify the `timezone` input to match your region:

  ```yaml
  with:
    timezone: "Europe/London"
  ```

---

## Reference

### Inputs

| Name              | Required | Description                                                            |
| ----------------- | -------- | ---------------------------------------------------------------------- |
| `holidays`        | ✅        | A comma-separated list of dates in `YYYY-MM-DD` format.                |
| `timezone`        | ❌        | IANA timezone name (e.g. `America/New_York`). Defaults to UTC.         |
| `fail-on-holiday` | ❌        | If `true`, fails the job when today is a holiday. Defaults to `false`. |

> [!NOTE]
> Any non-IANA timezone will be treated as UTC. If you need to use a specific timezone, ensure it is an IANA timezone name.

### Outputs

| Name         | Description                                                                   |
| ------------ | ----------------------------------------------------------------------------- |
| `on_holiday` | `"true"` if today is a holiday in the specified timezone, otherwise `"false"` |

---

## Explanation

### Why Use cron-holiday?

If your company avoids deployments or data processing on certain days, `cron-holiday` acts as a single gatekeeper. Instead of adding date checks to every workflow or manually enabling/disabling jobs, use this action to short-circuit jobs early and keep your CI/CD process clean and reliable.

You can set the holidays as a repository or organization level environment variable, or even use a shared configuration file to manage holidays across multiple workflows.

### Naming Note

 *"On holiday!"* - not just a practical check, but a nod to the [Green Day song](https://genius.com/Green-day-holiday-lyrics).
 We believe your CI deserves some punk rock energy, and you the day off.

## Default Holidays

If you do not specify the `holidays` input, it defaults to the USA Federal Holidays for 2026 and 2027, using the observed date when a holiday falls on a weekend:

| Holiday | 2026 | 2027 |
| --- | --- | --- |
| New Year's Day | 2026-01-01 | 2027-01-01 |
| Martin Luther King, Jr. Day | 2026-01-19 | 2027-01-18 |
| Washington's Birthday | 2026-02-16 | 2027-02-15 |
| Memorial Day | 2026-05-25 | 2027-05-31 |
| Juneteenth National Independence Day | 2026-06-19 | 2027-06-18 |
| Independence Day | 2026-07-03 | 2027-07-05 |
| Labor Day | 2026-09-07 | 2027-09-06 |
| Columbus Day | 2026-10-12 | 2027-10-11 |
| Veterans Day | 2026-11-11 | 2027-11-11 |
| Thanksgiving Day | 2026-11-26 | 2027-11-25 |
| Christmas Day | 2026-12-25 | 2027-12-24 |
| New Year's Day (2028, observed) | n/a | 2027-12-31 |
You can override this by specifying your own list of dates in the `holidays` input.
