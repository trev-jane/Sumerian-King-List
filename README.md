# Sumerian King List

This is a (currently ongoing) project I undertook to gain experience with databases (and an excuse to learn more about ancient Mesopotamian history,
a personal interest of mine). The project was done in SQLite with two related tables linked by foreign key.

## About the Source

The Sumerian King List is one of the oldest surviving historical documents. It is at times wildly unhinged and comically implausible, occasionally dryly funny, and always fascinating - a unique blend of myth, ancient religion, political propaganda, and genuine historical records.

## Database Schema

Two related tables, linked by foreign key:

**kings** — Tracks name, reign length, city, dynasty, pre/post-flood, sequence number, epithets, and historicity for each king, along with a comments section.

**cities** — The city-states that held kingship at various points, including location data and notes on archaeological attestation.

## King Notes

- Flood -
The Sumerian King List is separated into pre and post-flood kings, with eight kings having ruled before a great flood swept over the land. Each of the pre-flood kings is attributed a reign of tens of thousands of years, and none are archaeologically attested. The Sumerian flood tradition is the likely basis for the Biblical story of Noah, as they bear striking similarities. Flood traditions are extremely widespread in ancient civilizations, perhaps because civilizations generally form near rivers, which are known to flood.

- Historicity -
The historicity of each king is recorded as a rating between 1 and 5:
| Value | Meaning |
|-------|---------|
| 1 | Generally considered a purely mythological figure by historians |
| 2 | Believed likely to be a mythological figure by historians |
| 3 | Historicity unknown |
| 4 | Believed likely to be a historical figure with some archaeological evidence, perhaps with legendary tales attributed to them |
| 5 | Historical figure based on solid archaeological data, contemporary records, monuments, etc. |

It is worth noting that any and all of the kings on the list may refer to a real person who existed at one time, even if the details attributed to them are entirely implausible. Even legendary tales, such as Mesh-ki-ang-gasher who ruled for over three centuries before vanishing into the sea and ascending into the mountains, could derive from a real king who was lost at sea.

## Sample Queries

Average reign length before and after the flood:
```sql
SELECT 
    CASE WHEN pre_flood = 1 THEN 'Pre-flood' ELSE 'Post-flood' END AS period,
    AVG(reign) AS average_reign
FROM kings
GROUP BY pre_flood;
```

All kings with archaeological attestation:
```sql
SELECT name, dynasty, reign, epithet
FROM kings
WHERE historicity >= 4
ORDER BY sequence_number;
```

Kings and their cities joined:
```sql
SELECT kings.name, kings.reign, kings.dynasty, cities.name AS city
FROM kings
JOIN cities ON kings.city_id = cities.id
ORDER BY kings.sequence_number;
```

## Files

- `sumerian_kings.db` — the SQLite database
- `sumerian_kings.sql` — full dump; run against a fresh .db file to recreate the database from scratch
