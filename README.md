# mongoDB-validator
//validator

{"EMPLOYEE":{"eno":111,
			 "fname":"xxx",
			 "lname":"xxx"}}
			 
db.createCollection("key_name",{"validator":{$jsonSchema:{...}}})


//string
{"key_name":{"bsonType":"string"}}
//length restriction
{"key_name":{"bsonType":"string", "minLength":5, "maxLength":30}}
//number restriction in string
{"key_name":{"bsonType":"string", "pattern":"[1-9][0-9][0-9]"}} //"100" to "999" 
{"key_name":{"bsonType":"string", "pattern":"[a-z][A-Z][0-9]"}}

//date
{"key_name":{"bsonType":"date"}} //"YYYY-MM-DD" when insert:new Date("YYYY-MM-DD")

//numerical
//integer
{"key_name":{"bsonType":"int"}}
//decimal
{"key_name":{"bsonType":"double"}}
//multiple of a integer
{"key_name":{"bsonType":"int", "multipleOf":2}} //2x, x = {0 to inf}
//multiple of a decimal
{"key_name":{"bsonType":"double", "multipleOf":0.2}} //0.2x, x = {0 to inf}
//range of a integer
{"key_name":{"bsonType":"int", "minimum":2, "maximum":10}} // 2 <= x <= 10
{"key_name":{"bsonType":"int", "exclusiveMinimum":2, "exclusiveMaximum":10}} // 2 < x < 10

//boolean
{"key_name":{"bsonType":"boolean"}}

//null
{"key_name":{"bsonType":"null"}}

//object
{"key_name":{"bsonType":"object",
			 "properties":{"key1":{"bsonType":{"...."}},
						   "key2":{"bsonType":{"...."}},
						   "key3":{"bsonType":{"...."}},
			 "required":["key1", "key2"]}}
//{"key_name":{"key1":xxx, "key2":xxx}} can pass the validator
//{"key_name":{"key1":xxx, "key2":xxx, "key3":xxx}} can pass the validator
//{"key_name":{"key1":xxx, "key3":xxx}} cannot pass the validator
{"key_name":{"bsonType":"object",
			 "properties":{"key1":{"bsonType":{"...."}},
						   "key2":{"bsonType":{"...."}},
						   "key3":{"bsonType":{"...."}}},
			 "required":["key1", "key2"],
			 "additionalProperties":false,
			 "minProperties":2,
			 "maxProperties":3}}
			 
//array
{"key_name":{"bsonType":"array", "items":{"bsonType":"..."}}}
{"key_name":{"bsonType":"array", "items":{"bsonType":"..."}, "uniqueItems":true, "minItem":3, "maxItems":5}}

{"key_name":{"bsonType":"array", "items":{"bsonType":"object",
										  "properties":{},
										  "requires":[]}}}


db.collection.insert({"cname":"company1",
		"city":"Wollongong",
		"street":"xxxx",
		"bldg":10,
		"budget":4500.0,
		"departments":[{"dname":"department1",
						"floor":[1, 2, 3],
						"employees":[{"enumber":"1001",
									  "fname":"Alex",
									  "lname":"Sue",
									  "date_of_birth":new Date("2000-01-01"),
									  "position":"manager",
									  "salary":50.0}]},
						]});





db.collection_name.drop()
db.createCollection(
	{"Company",
	{"validator":{$jsonSchema:{"bsonType":"object",
							   "required":["cname", "city", "street", "bldg", "budget", "departments"],
							   "properties":{"cname":{"bsonType":"string"},
											 "city":{"bsonType":"string"},
											 "street":{"bsonType":"string"},
											 "bldg":{"bsonType":"int"},
											 "budget":{"bsonType":"double"},
											 "departments":{"bsonType":"array", 
															"items":{"bsonType":"object",
																	 "required":["dname", "floor", "employees"],
																	 "properties":{"dname":{"bsonType":"string"},
																				   "floor":{"bsonType":"array",
																							"items":{"bsonType":"int"}},
																				   "employees":{"bsonType":"array",
																								"items":{"bsonType":"object",
																										 "required":["enumber", "first_name", "last_name", "date_of_birth", "salary"],
																										 "properties":{"enumber":{"bsonType":"string"},
																													   "first_name":{"bsonType":"string"},
																													   "last_name":{"bsonType":"string"},
																													   "date_of_birth":{"bsonType":"string",
																																		"format":"date"},
																													   "position":{"bsonType":"string"},
																													   "salary":{"bsonType":"double"}
																													  }
																										 }
																				               }
																				  }
																	}
															}
											}
							   }
				  }
	}
	}
);
	
	
	
	
	
	
	
	
	
	
	
	
	








