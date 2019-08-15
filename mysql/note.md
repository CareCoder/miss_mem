#### 避免group by 连接 order by
```sql
SELECT
	*
FROM
	hy_alarm_send_message
WHERE
	id IN (
		SELECT
			MAX(t.id) AS max_id
		FROM
			hy_alarm_send_message t
		JOIN (
			SELECT
				MAX(create_time) AS max_create_time,
				alarm_id
			FROM
				hy_alarm_send_message
			GROUP BY
				alarm_id
		) t1 ON t.alarm_id = t1.alarm_id
		AND t.create_time = t1.max_create_time
		GROUP BY
			create_time
	)

```
