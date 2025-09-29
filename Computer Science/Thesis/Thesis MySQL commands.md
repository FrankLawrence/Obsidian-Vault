---
Tags: 
Created: 2025-05-24 20:31:48
---
(Links:: [[Bachelor Thesis Computer Science VU]])
This page outlines some useful commands that I have used for my Bachelor Thesis, or ones that I have used.

```mysql
CREATE TABLE nameservers (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nameserver VARCHAR(255) NOT NULL UNIQUE,
  ip_nameserver VARCHAR(45),  -- Supports both IPv4 & IPv6
  owner_nameserver VARCHAR(255),
  country_nameserver VARCHAR(10)
);
```
```mysql
CREATE TABLE tranco_nameservers (
  domain_id INT NOT NULL,
  nameserver_id INT NOT NULL,
  PRIMARY KEY (domain_id, nameserver_id),
  FOREIGN KEY (domain_id) REFERENCES tranco(id) ON DELETE CASCADE,
  FOREIGN KEY (nameserver_id) REFERENCES nameservers(id) ON DELETE CASCADE
);
```
```
num_dist_nameservers=$(mysql_query "
SELECT
    COUNT(DISTINCT nameserver)
FROM nameservers;")
```

```
num_domains_openintel=$(mysql_query "
SELECT
    COUNT(DISTINCT query_name)
FROM openintel;")
```
```
num_domains_openintel_rrsig=$(mysql_query "
SELECT
    COUNT(DISTINCT query_name)
FROM openintel
WHERE rrsig_signature IS NOT NULL;")
```

```
num_gov_domains=$(mysql_query "
SELECT
    COUNT(DISTINCT query_name)
FROM gov_domains;")
```
```
num_gov_domains_rrsig=$(mysql_query "
SELECT
COUNT(DISTINCT query_name)
FROM gov_domains
WHERE rrsig_signature IS NOT NULL;")
```

```
query="
SELECT
    COUNT(*) AS 'Count (domains)',
    nameserver AS Nameserver
FROM tranco AS t
JOIN tranco_nameservers AS tn ON domain_id=t.id
JOIN nameservers AS n ON nameserver_id=n.id
GROUP BY nameserver
ORDER BY COUNT(*) DESC
LIMIT 10;"
```
```
top_10_nameservers_tranco=$(mysql_query "
SELECT
    COUNT(*) AS 'Count (domains)',
    nameserver AS Nameserver,
    owner_nameserver AS Owner
FROM tranco AS t
JOIN tranco_nameservers AS tn ON domain_id=t.id
JOIN nameservers AS n ON nameserver_id=n.id
GROUP BY nameserver, owner_nameserver
ORDER BY COUNT(*) DESC
LIMIT 10;")
```

```
# top_10_asn_openintel=$(mysql_query "
# SELECT
#     COUNT(DISTINCT query_name) AS 'COUNT (domains)',
#     `as` AS ASN
# FROM openintel
# WHERE `as` IS NOT NULL
# GROUP BY `as`
# ORDER BY COUNT(*) DESC
# LIMIT 10") &
```

```
num_dns_resolvers=$(mysql_query "
SELECT
    COUNT(*)
FROM dns_resolvers;")
top_10_country_resolvers=$(mysql_query "
SELECT
   COUNT(*) AS 'Count (DSN Resolvers)',
   geo_location AS COUNTRY
FROM dns_resolvers AS d
GROUP BY geo_location
ORDER BY COUNT(*) DESC
LIMIT 10;")
```

```
num_nonvalidated_openintel=$(mysql_query "
SELECT
COUNT(DISTINCT query_name)
FROM openintel
WHERE ad_flag IS false;")
```
```
num_validated_openintel=$(mysql_query "
SELECT
    COUNT(DISTINCT query_name)
FROM openintel
WHERE ad_flag IS true;")
```
```
num_nonvalidated_gov=$(mysql_query "
SELECT
    COUNT(DISTINCT query_name)
FROM gov_domains
WHERE ad_flag IS false;")
```
```
num_validated_gov=$(mysql_query "
SELECT
    COUNT(DISTINCT query_name)
FROM gov_domains
WHERE ad_flag IS true;")
```

```
num_domain_and_ns_owner=$(mysql_query "
SELECT
    COUNT(*)
FROM tranco AS t
JOIN tranco_nameservers AS tn ON domain_id=t.id
JOIN nameservers AS n ON nameserver_id=n.id
WHERE owner=n.owner_nameserver;")
```


---
References: