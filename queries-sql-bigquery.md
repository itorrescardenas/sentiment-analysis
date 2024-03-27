#### count null values
```sql
--- table amazon_product
SELECT
COUNTIF(product_id IS NULL) AS product_id_nulo,
COUNTIF(product_name IS NULL) AS product_name_nulo,
COUNTIF(category IS NULL) AS category_nulo,
COUNTIF(discounted_price IS NULL) AS discounted_price_nulo,
COUNTIF(actual_price IS NULL) AS actual_price_nulo,
COUNTIF(discount_percentage IS NULL) AS discount_percentage_
COUNTIF(about_product IS NULL) AS about_product_nulo
FROM `proyecto4-amazon-416316.dataset4.amazon_product`
--- table amazon_review
SELECT
COUNTIF(user_id IS NULL) AS user_id_nulo,
COUNTIF(user_name IS NULL) AS user_name_nulo,
COUNTIF(review_id IS NULL) AS review_id_nulo,
COUNTIF(review_title IS NULL) AS review_title_nulo,
COUNTIF(review_content IS NULL) AS review_content_nulo,
COUNTIF(product_id IS NULL) AS product_id_nulo,
COUNTIF(rating IS NULL) AS rating_nulo,
COUNTIF(rating_count IS NULL) AS rating_count_nulo
FROM `proyecto4-amazon-416316.dataset4.amazon_review`
```
#### product_review clean
```sql
WITH
review_limpia AS (
SELECT
user_id,
REGEXP_REPLACE(user_name, r'[^a-zA-Z ]', '') AS user_name
review_id,
REGEXP_REPLACE(review_title, r'[^a-zA-Z ]', '') AS review
review_content,
IF
(COALESCE(img_link, '') <> '', 1, 0) AS img_link,
IF
(COALESCE(product_link, '') <> '', 1, 0) AS product_link
product_id,
ROW_NUMBER() OVER (PARTITION BY product_id ORDER BY prod
CAST(REPLACE(rating, '|', '3.9') AS FLOAT64) AS rating,
IFNULL(rating_count, 0) AS rating_count
FROM
`proyecto4-amazon-416316.dataset4.amazon_review` )
SELECT
* EXCEPT (row_num)
FROM
review_limpia
WHERE
row_num = 1;

```
#### table join
```sql
SELECT
a.*,
b.* EXCEPT (product_id)
FROM
proyecto4-amazon-416316.dataset4.amazon_product_limpia AS a
LEFT JOIN
proyecto4-amazon-416316.dataset4.product_review_limpio AS b
ON
a.product_id = b.product_id
```
#### correlation between two variables
```sql
SELECT
CORR(discounted_price,rating) AS correlacion,
CORR(rating_count, rating) AS correlacion2,
CORR(pos,discounted_price) AS correlacion3,
CORR(neg,discounted_price) AS correlacion4
FROM `proyecto4-amazon-416316.dataset4.amazon_limpia_puntajes
```


