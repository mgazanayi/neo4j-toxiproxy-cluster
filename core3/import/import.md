CREATE INDEX ON :Recipe(id);
CREATE INDEX ON :Ingredient(name);
CREATE INDEX ON :Keyword(name);
CREATE INDEX ON :DietType(name);
CREATE INDEX ON :Author(name);
CREATE INDEX ON :Collection(name);
:params jsonFile => "https://raw.githubusercontent.com/mneedham/bbcgoodfood/master/stream_all.json";
CALL apoc.load.json($jsonFile) YIELD value
WITH value.page.article.id AS id,
       value.page.title AS title,
       value.page.article.description AS description,
       value.page.recipe.cooking_time AS cookingTime,
       value.page.recipe.prep_time AS preparationTime,
       value.page.recipe.skill_level AS skillLevel
MERGE (r:Recipe {id: id})
SET r.cookingTime = cookingTime,
    r.preparationTime = preparationTime,
    r.name = title,
    r.description = description,
    r.skillLevel = skillLevel;
CALL apoc.load.json($jsonFile) YIELD value
WITH value.page.article.id AS id,
       value.page.article.author AS author
MERGE (a:Author {name: author})
WITH a,id
MATCH (r:Recipe {id:id})
MERGE (a)-[:WROTE]->(r);
CALL apoc.load.json($jsonFile) YIELD value
WITH value.page.article.id AS id,
       value.page.recipe.ingredients AS ingredients
MATCH (r:Recipe {id:id})
FOREACH (ingredient IN ingredients |
  MERGE (i:Ingredient {name: ingredient})
  MERGE (r)-[:CONTAINS_INGREDIENT]->(i)
);
CALL apoc.load.json($jsonFile) YIELD value
WITH value.page.article.id AS id,
       value.page.recipe.keywords AS keywords
MATCH (r:Recipe {id:id})
FOREACH (keyword IN keywords |
  MERGE (k:Keyword {name: keyword})
  MERGE (r)-[:KEYWORD]->(k)
);
CALL apoc.load.json($jsonFile) YIELD value
WITH value.page.article.id AS id,
       value.page.recipe.diet_types AS dietTypes
MATCH (r:Recipe {id:id})
FOREACH (dietType IN dietTypes |
  MERGE (d:DietType {name: dietType})
  MERGE (r)-[:DIET_TYPE]->(d)
);
CALL apoc.load.json($jsonFile) YIELD value
WITH value.page.article.id AS id,
       value.page.recipe.collections AS collections
MATCH (r:Recipe {id:id})
FOREACH (collection IN collections |
  MERGE (c:Collection {name: collection})
  MERGE (r)-[:COLLECTION]->(c)
);