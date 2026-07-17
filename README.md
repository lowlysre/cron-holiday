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
         holidays: "2025-01-01,2025-07-04,2025-12-25"
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

If you do not specify the `holidays` input, it defaults to the USA Federal Holidays for 2025:

- 2025-01-01 (New Year's Day)
- 2025-01-20 (Martin Luther King, Jr. Day)
- 2025-02-17 (Washington’s Birthday)
- 2025-05-26 (Memorial Day)
- 2025-06-19 (Juneteenth National Independence Day)
- 2025-07-04 (Independence Day)
- 2025-09-01 (Labor Day)
- 2025-10-13 (Columbus Day)
- 2025-11-11 (Veterans Day)
- 2025-11-27 (Thanksgiving Day)
- 2025-12-25 (Christmas Day)

You can override this by specifying your own list of dates in the `holidays` input.
