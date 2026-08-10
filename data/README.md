# Data

Five CSV files, covering **500 Megaline clients** and their **2018 usage activity**. All files are required to run the notebook end-to-end and are included in this folder.

---

### `megaline_users.csv` — one row per customer

| Column | Description |
|---|---|
| `user_id` | Unique customer identifier |
| `first_name`, `last_name` | Customer name |
| `age` | Customer age (years) |
| `city` | Customer's metro area (used to identify the NY-NJ region for the second hypothesis test) |
| `reg_date` | Subscription start date |
| `plan` | Plan name (`surf` or `ultimate`) |
| `churn_date` | Date the customer stopped using the service; **missing = still active**, not an error |

---

### `megaline_calls.csv` — one row per call

| Column | Description |
|---|---|
| `id` | Unique call identifier |
| `user_id` | Customer who made the call |
| `call_date` | Date of the call |
| `duration` | Call duration in decimal minutes. **Billing rule:** each individual call is rounded **up** to the next full minute, even a 1-second call. |

---

### `megaline_messages.csv` — one row per SMS

| Column | Description |
|---|---|
| `id` | Unique message identifier |
| `user_id` | Customer who sent the message |
| `message_date` | Date the message was sent |

---

### `megaline_internet.csv` — one row per internet session

| Column | Description |
|---|---|
| `id` | Unique session identifier |
| `user_id` | Customer who used the session |
| `session_date` | Date of the session |
| `mb_used` | Data used in that session (MB). **Billing rule:** individual sessions are *not* rounded — only the **monthly total** is rounded up to the next whole GB. |

---

### `megaline_plans.csv` — one row per plan

| Column | Description |
|---|---|
| `plan_name` | Plan identifier (`surf` or `ultimate`) |
| `usd_monthly_pay` | Fixed monthly subscription fee |
| `minutes_included` | Minutes included in the monthly fee |
| `messages_included` | Text messages included in the monthly fee |
| `mb_per_month_included` | Data included in the monthly fee (MB) |
| `usd_per_minute` | Overage price per extra minute |
| `usd_per_message` | Overage price per extra message |
| `usd_per_gb` | Overage price per extra GB |

### Plan comparison

| Feature | Surf | Ultimate |
|---|---:|---:|
| Monthly fee | $20 | $70 |
| Included minutes | 500 | 3000 |
| Included messages | 50 | 1000 |
| Included data | 15 GB | 30 GB |
| Extra minute | $0.03 | $0.01 |
| Extra message | $0.03 | $0.01 |
| Extra GB | $10 | $7 |

---

## Notes

- Zero-duration calls (~19.5% of `megaline_calls.csv`) and zero-usage internet sessions (~13.1% of `megaline_internet.csv`) are **retained**, not dropped — they represent valid call/session attempts and do not distort the revenue calculation once Megaline's rounding rules are applied.
- All monetary values are in **USD**.
