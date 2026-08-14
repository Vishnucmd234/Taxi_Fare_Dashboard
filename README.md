# Taxi Fare Dashboard — Analysis Report

**Project:** Taxi Fare Dashboard
**Tool:** Power BI
**Repo:** [github.com/Vishnucmd234/Taxi_Fare_Dashboard](https://github.com/Vishnucmd234/Taxi_Fare_Dashboard/tree/main)
**Pages:** Overall | Vehicle Type | Cancellation | Rating | Summary

---

## 1. Overview

This dashboard tracks ride bookings for a multi-vehicle taxi/mobility platform (Auto, Bike, eBike, Go Mini, Go Sedan, Premier Sedan, Uber XL) across January–May 2024. It's built as a five-page interactive report covering booking volume and status, vehicle-level performance, cancellation behavior, customer/driver ratings, and a revenue/KPI summary.

![Home Page](image/HOME.png)
![Overall Page](image/OVERALL.png)
![Vehicle Type Page](image/VAHICAL.png)
![Cancellation Page](image/CANCELLATION.png)
![Rating Page](image/RATING.png)
![Summary Page](image/SUMMARY.png)

*(Add the five screenshots to an `/images` folder in the repo so they render above.)*

---

## 2. Page-by-Page Breakdown

### Overall
- Total bookings sit around **59K**, with a booking status split of: **Completed 62.13%**, **Cancelled by Driver 18.09%**, **No Driver Found 6.94%**, **Cancelled by Customer 6.93%**, and **Incomplete 5.91%**.
- The monthly trend line (Jan–May) is fairly flat through Q1 (~12–13K bookings/month) before dropping sharply into April and May — worth flagging since May's data may be a partial month rather than a true decline.

### Vehicle Type
- **Uber XL** leads in raw volume (13M total booking value, 12M successful), followed by **Go Mini** (10M/9M) and **eBike** (9M/9M).
- **Auto** is the clear laggard: only 2M total value converting to 1M successful — roughly a **50% success rate**, far below every other category.
- **eBike** and **Bike** convert almost 1:1 (total ≈ success), suggesting these are the most reliably fulfilled ride types, even though they're not the top revenue generators.
- Average distance travelled is tightly clustered around **26 km** across all vehicle types — this is a fairly homogeneous distance profile, not a segment where short-hop vs long-haul vehicles are differentiated.

### Cancellation
- Cancelled-by-customer volume (84.969K, 52.647K in the KPI cards) is presented alongside a **~25% cancellation rate**.
- The two reason-breakdown pie charts (customer cancellation reasons, driver cancellation reasons) show perfectly even splits — 20% each across 5 customer reasons, 25% each across 4 driver reasons. That even split is a signal worth double-checking in the data model (see Insight #5 below).

### Rating
- Ratings are shown by vehicle type as aggregated volumes rather than average scores (e.g., Uber XL: 101.93K customer / 98.00K driver).
- Every single vehicle type shows **driver rating slightly below customer rating**, with a highly consistent gap (~2–4K, or roughly 2-4%).

### Summary
- **Customer Retention Rate: 1.75%**
- **Revenue per KM (sum): 148.28K**
- Revenue by distance category peaks at **Medium**, ahead of Short, Long, and Extra Long.
- Booking value is heavily concentrated in the **Premium** revenue tier, dwarfing High/Medium/Low.
- Evening is the strongest day-part for booking value, followed closely by Morning and Afternoon, with Night trailing well behind.
- The Service Quality Score gauge reads **77.45 out of a 154.90 max** — effectively landing at the midpoint of its scale.

---

## 3. Insights (Deep-Dive)

**1. Driver-side supply is the primary bottleneck, not customer demand.**
Cancelled-by-driver (18.09%) is nearly 3x higher than cancelled-by-customer (6.93%), and "No Driver Found" adds another 6.94% on top. Combined, driver-availability issues account for roughly a **quarter of all non-completed bookings** — more than double the customer-side cancellation rate. This points to a supply/dispatch problem (driver shortage, poor matching, or drivers declining low-value trips) rather than a demand-quality problem. Any retention or revenue initiative should prioritize fixing driver fulfillment before optimizing marketing spend to acquire more riders.

**2. Auto is structurally underperforming and dragging down platform reliability.**
At a ~50% success rate, Auto converts roughly half as reliably as every other vehicle category (all of which sit at 75–100%). Since average distance travelled is nearly identical across vehicle types (~26 km), the gap isn't a route-length issue — it's likely driver supply, pricing mismatch, or route/zone coverage specific to Autos. Given Auto also has the smallest total booking value (2M), it's a small-but-broken segment: worth investigating whether it's worth fixing or worth deprioritizing in favor of scaling Go Mini/eBike, which convert almost perfectly.

**3. Retention is critically low relative to booking volume — the business is running on constant new-customer acquisition.**
A 1.75% customer retention rate against ~59K bookings suggests the platform is not building a repeat-rider base; nearly every trip is coming from a new or non-returning customer. Combined with Premium-tier revenue concentration and Evening being the peak revenue day-part, this looks like a platform that converts well on high-value, one-off occasion rides (e.g., evening outings) but fails to convert riders into habitual users. This is a much bigger long-term risk than the cancellation rate — acquisition-dependent growth is expensive and doesn't compound.

**4. Driver ratings are consistently and uniformly lower than customer ratings — a systemic pattern, not a vehicle-specific one.**
Every vehicle type without exception shows driver ratings trailing customer ratings by a similar margin. Because this gap is uniform across such different vehicle categories (bikes vs. sedans vs. XL), it's unlikely to be caused by vehicle-specific service issues. More plausible explanations: riders are more lenient/generous raters than drivers rating riders, or there's a structural bias in how the rating prompts are framed/timed. This is a useful pattern to validate against raw (non-aggregated) rating distributions rather than volumes, since the current values are sums, not averages, and sums are driven heavily by booking volume rather than sentiment.

**5. The cancellation-reason charts likely reflect a data granularity issue, not real-world behavior.**
Both cancellation-reason pie charts show perfectly even splits (20% × 5 reasons for customers, 25% × 4 reasons for drivers). Real-world cancellation reasons are almost never evenly distributed — this pattern usually indicates the visual is plotting a **distinct count of reason categories** rather than the **actual count of cancelled rides per reason**. Before using this chart to make operational decisions (e.g., "AC not working" is a top cancellation driver), the underlying measure should be re-checked — it's currently reporting "1 (20%)" per slice, which reads like a count-of-rows artifact rather than a true volume-weighted breakdown.

**6. Revenue concentration in Medium-distance, Premium-tier, Evening rides suggests an opportunity to rebalance pricing/dispatch around off-peak and non-Premium segments.**
With Short and Medium distance categories generating the bulk of per-KM revenue (ahead of Long/Extra Long), and Evening dominating booking value, the platform's revenue engine is narrow: short-to-medium trips, in the evening, from Premium-tier customers. Diversifying demand — e.g., incentivizing Morning/Afternoon usage or improving conversion in Medium/Low revenue tiers — would reduce dependency on a single time-of-day and customer-tier combination, and would also help smooth out the driver-supply strain that's likely worse during the Evening peak.

---

## 4. Suggested Next Steps
- Re-audit the cancellation-reason measures (row count vs. weighted count) before drawing conclusions from those charts.
- Break out ratings as **averages** (not sums) per vehicle type to separate "popular vehicle" from "well-rated vehicle."
- Investigate Auto's low success rate at the zone/driver-supply level.
- Build a retention cohort view (new vs. repeat riders over time) to complement the single 1.75% headline figure.
