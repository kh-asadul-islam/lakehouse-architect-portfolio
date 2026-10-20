|name|expression|why|rows_failed|
|---|---|---|---|
|fare_non_negative|fare_amount >= 0|A fare is a charge. Negative fares are refunds or voided meters and would drag averages negative.|405217|
|total_non_negative|total_amount >= 0|Total is what the rider actually pays; negative totals are disputed or cancelled rides, not revenue.|188712|
|pickup_before_dropoff|pickup_ts < dropoff_ts|A trip cannot end before it starts; reversed timestamps produce negative trip_minutes and poison duration and speed analytics.|8438|
|duration_sane|trip_minutes BETWEEN 0 AND 1440|Trips longer than 24 hours indicate a stuck meter; 1440 minutes is the longest plausible shift.|370|
|distance_sane|trip_distance >= 0 AND trip_distance < 200|The NYC taxi zone map spans roughly 30 miles end to end; 200 is generous headroom. Negative distances are meter errors.|364|
|pickup_in_window|pickup_ts >= 2024-12-01 AND pickup_ts < 2025-03-01|Guards the declared load window (Dec 2024 - Feb 2025). TLC files contain stray boundary rows with impossible dates; anything outside the window is source junk.|32|
|passengers_sane|passenger_count IS NULL OR passenger_count BETWEEN 0 AND 8|Yellow cabs legally seat about 6 (8 = headroom). NULL is allowed because some vendor files omit passenger_count entirely.|8|
|zones_valid|pu_location_id BETWEEN 1 AND 265 AND do_location_id BETWEEN 1 AND 265|The TLC zone lookup only defines LocationIDs 1-265. Anything outside silently drops rows in joins with taxi_zones (L06/L07).|0|