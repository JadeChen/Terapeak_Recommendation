Sqlite Database: AmazonRecommendation.db

Table: Amazon_Recommendation
Data Structure:
	CREATE TABLE Amazon_Recommendation (
    slr_id                   VARCHAR (32)   NOT NULL,
    slr_item_id              VARCHAR (32)   NOT NULL,
    rec_item_id              VARCHAR (32)   NOT NULL,
    similarity               DECIMAL (4, 0),
    similarProduct           BOOLEAN,
    similarViewedProduct     BOOLEAN,
    otherCategSimilarProduct BOOLEAN,
    FOREIGN KEY (
        slr_item_id
    )
    REFERENCES Amazon_ItemInfo (item_id),
    FOREIGN KEY (
        rec_item_id
    )
    REFERENCES Amazon_ItemInfo (item_id),
    PRIMARY KEY (
        slr_id,
        slr_item_id,
        rec_item_id
    )
);
Specification:
	similarity: the score that specify how similar the two items' titles are
	- The value of similarity would be used for de-duplicating items by SQL:
		SELECT slr_item_id, slr_item_id FROM Amazon_Recommendation WHERE slr_id = <your slr_id> AND similarity < <your threshold>;
	
	similarProduct, similarViewedProduct, otherCategSimilarProduct are three types of Amazon Recommendations:
		similarProduct: Customers who bought this [item] also bought
		similarViewedProduct: Customers who viewed this [item] also viewed
		otherCategSimilarProduct: other categories that are similar to the item specified in the request
	-The value of them can only be set to 1 or 0:
		1 - the item belongs to that type
		0 - not belongs to that type
	-It is possible that one item belongs to multiple types.
	
	- The slr_item_id and rec_item_id refer to the item_id in Table Amazon_ItemInfo. So after retrieving the id of recommended items, you can find its attributes from that table.

	
Table: Amazon_ItemInfo
Data Structure:
	CREATE TABLE Amazon_ItemInfo (
    item_id          VARCHAR (32)    NOT NULL
                                     PRIMARY KEY,
    item_title       VARCHAR (255),
    offer_price      DECIMAL (15, 2),
    currency_code    VARCHAR (32),
    product_group    VARCHAR (255),
    product_typename VARCHAR (255),
    LNP_price        DECIMAL (15, 2),
    LNP_amount       DECIMAL (9, 0),
    M_image          VARCHAR (256),
    L_image          VARCHAR (256),
    sales_rank       DECIMAL (9, 0),
    categ_id         VARCHAR (32),
    categ_name       VARCHAR (255) 
);
Specification:
	LNP_price: LowestNewPrice
	LNP_amount: You can ignore this attribute, since I find it is just LNP_price*100
	sales_rank: a large number means the item has NOT sold well
	categ_id, categ_name: it is the root category that the item belongs to
	
	




