``` sql

CREATE TABLE "auth_group" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"name" varchar(150) NOT NULL UNIQUE);

CREATE TABLE "auth_group_permissions" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"group_id" integer NOT NULL REFERENCES "auth_group" ("id") DEFERRABLE INITIALLY DEFERRED, 
"permission_id" integer NOT NULL REFERENCES "auth_permission" ("id") DEFERRABLE INITIALLY DEFERRED);

CREATE TABLE "auth_permission" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"content_type_id" integer NOT NULL REFERENCES "django_content_type" ("id") DEFERRABLE INITIALLY DEFERRED, 
"codename" varchar(100) NOT NULL, 
"name" varchar(255) NOT NULL);

CREATE TABLE "auth_user" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"password" varchar(128) NOT NULL, 
"last_login" datetime NULL, 
"is_superuser" bool NOT NULL, 
"username" varchar(150) NOT NULL UNIQUE, 
"last_name" varchar(150) NOT NULL, 
"email" varchar(254) NOT NULL, 
"is_staff" bool NOT NULL, 
"is_active" bool NOT NULL, 
"date_joined" datetime NOT NULL, 
"first_name" varchar(150) NOT NULL);

CREATE TABLE "auth_user_groups" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"user_id" integer NOT NULL REFERENCES "auth_user" ("id") DEFERRABLE INITIALLY DEFERRED, 
"group_id" integer NOT NULL REFERENCES "auth_group" ("id") DEFERRABLE INITIALLY DEFERRED);

CREATE TABLE "auth_user_user_permissions" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"user_id" integer NOT NULL REFERENCES "auth_user" ("id") DEFERRABLE INITIALLY DEFERRED, 
"permission_id" integer NOT NULL REFERENCES "auth_permission" ("id") DEFERRABLE INITIALLY DEFERRED);

CREATE TABLE "created_profile_by_user" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"creator_id" integer NOT NULL REFERENCES "profile" ("id") DEFERRABLE INITIALLY DEFERRED, 
"first_name" varchar(512) NOT NULL, 
"last_name" varchar(512) NOT NULL, 
"middle_name" varchar(512) NOT NULL);

CREATE TABLE "dictation" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"start_time" time NOT NULL, 
"end_time" time NOT NULL, 
"is_registration_open" bool NOT NULL, 
"difficulty" integer NOT NULL,
"event_id" integer NOT NULL REFERENCES "event" ("id") DEFERRABLE INITIALLY DEFERRED, 
"space_id" integer NOT NULL REFERENCES "space" ("id") DEFERRABLE INITIALLY DEFERRED, 
"office" varchar(256) NOT NULL, 
"number_of_seats" integer NOT NULL, 
"short_title" varchar(512) NOT NULL, 
"illustrator" varchar(512) NOT NULL);


CREATE TABLE "django_admin_log" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"action_time" datetime NOT NULL, 
"object_id" text NULL, 
"object_repr" varchar(200) NOT NULL, 
"change_message" text NOT NULL, 
"content_type_id" integer NULL REFERENCES "django_content_type" ("id") DEFERRABLE INITIALLY DEFERRED, 
"user_id" integer NOT NULL REFERENCES "auth_user" ("id") DEFERRABLE INITIALLY DEFERRED, 
"action_flag" smallint unsigned NOT NULL CHECK ("action_flag" >= 0));

CREATE TABLE "django_content_type" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"app_label" varchar(100) NOT NULL, 
"model" varchar(100) NOT NULL);

CREATE TABLE "django_migrations" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT,
"app" varchar(255) NOT NULL, 
"name" varchar(255) NOT NULL, 
"applied" datetime NOT NULL);

CREATE TABLE "django_session" 
("session_key" varchar(40) NOT NULL PRIMARY KEY, 
"session_data" text NOT NULL, 
"expire_date" datetime NOT NULL);

CREATE TABLE "event" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"title" varchar(512) NOT NULL, 
"date" date NOT NULL, 
"creator_id" integer NOT NULL REFERENCES "profile" ("id") DEFERRABLE INITIALLY DEFERRED, 
"description" varchar(2048) NOT NULL);

CREATE TABLE "federal_partner" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT,
"title" varchar(512) NOT NULL, 
"logo" varchar(100) NOT NULL, 
"link" varchar(1024) NOT NULL);

CREATE TABLE "main_team" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT,
"vacancy" varchar(512) NOT NULL, 
"profile_id" integer NOT NULL REFERENCES "profile" ("id") DEFERRABLE INITIALLY DEFERRED);

CREATE TABLE "news" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"title" varchar(512) NOT NULL, 
"content" text NOT NULL,
"created_at" datetime NOT NULL, 
"is_main_feed" bool NOT NULL, 
"is_for_region" bool NOT NULL, 
"author_id" integer NOT NULL REFERENCES "profile" ("id") DEFERRABLE INITIALLY DEFERRED, 
"region_id" integer NULL REFERENCES "region" ("id") DEFERRABLE INITIALLY DEFERRED);

CREATE TABLE "profile" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"code" integer NOT NULL, 
"is_confirm" bool NOT NULL,
"role" varchar(256) NOT NULL, 
"photo" varchar(100) NULL, 
"country" varchar(256) NULL, 
"city" varchar(512) NULL, 
"middle_name" varchar(512) NULL, 
"phone" varchar(256) NULL, 
"education" varchar(1024) NULL, 
"age" varchar(256) NULL, 
"sex" varchar(256) NULL, 
"second_email" varchar(256) NULL, 
"second_email_code" integer NULL,
"is_second_email_general" bool NOT NULL,
"is_second_email_approved" bool NOT NULL, 
"user_id" integer NOT NULL REFERENCES "auth_user" ("id") DEFERRABLE INITIALLY DEFERRED, 
"secret_word" varchar(512) NOT NULL);

CREATE TABLE "region" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT,
"region" varchar(512) NOT NULL, 
"photo" varchar(100) NULL);

CREATE TABLE "region_partner" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"name" varchar(512) NOT NULL,
"link" varchar(1024) NULL, 
"photo" varchar(100) NULL, 
"region_id" integer NOT NULL REFERENCES "region" ("id") DEFERRABLE INITIALLY DEFERRED);

CREATE TABLE "request_to_dictation" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"dictation_id" integer NOT NULL REFERENCES "dictation" ("id") DEFERRABLE INITIALLY DEFERRED, 
"status" varchar(256) NOT NULL, 
"first_name_of_created_by_user_profile" varchar(512) NOT NULL, 
"is_profile_created_by_user" bool NOT NULL, 
"last_name_of_created_by_user_profile" varchar(512) NOT NULL, 
"middle_name_of_created_by_user_profile" varchar(512) NOT NULL, 
"creator_id" integer NULL REFERENCES "profile" ("id") DEFERRABLE INITIALLY DEFERRED,
"profile_id" integer NULL REFERENCES "profile" ("id") DEFERRABLE INITIALLY DEFERRED);

CREATE TABLE "space" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT,
"address" varchar(512) NOT NULL, 
"country" varchar(256) NOT NULL, 
"city" varchar(256) NOT NULL,
"coords_longitude" real NOT NULL, 
"coords_latitude" real NOT NULL, 
"long_title" varchar(512) NOT NULL, 
"photo" varchar(100) NULL, 
"short_title" varchar(256) NOT NULL, 
"code" varchar(256) NOT NULL);

CREATE TABLE "space_in_region" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT,
"region_id" integer NOT NULL REFERENCES "region" ("id") DEFERRABLE INITIALLY DEFERRED, 
"space_id" integer NOT NULL REFERENCES "space" ("id") DEFERRABLE INITIALLY DEFERRED);

CREATE TABLE sqlite_sequence(name,seq);
CREATE TABLE "team_of_space"
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT,
"space_id" integer NOT NULL REFERENCES "space" ("id") DEFERRABLE INITIALLY DEFERRED,
"vacancy" varchar(512) NOT NULL, 
"name" varchar(512) NOT NULL, 
"teammate_id" integer NULL REFERENCES "profile" ("id") DEFERRABLE INITIALLY DEFERRED);

CREATE TABLE "user_in_region" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"profile_id" integer NOT NULL REFERENCES "profile" ("id") DEFERRABLE INITIALLY DEFERRED, 
"region_id" integer NOT NULL REFERENCES "region" ("id") DEFERRABLE INITIALLY DEFERRED);

CREATE TABLE "work_for_dictation" 
("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"email_for_work" varchar(512) NOT NULL, 
"first_name" varchar(512) NOT NULL, 
"last_name" varchar(512) NOT NULL, 
"secret_word" varchar(512) NOT NULL, 
"work" varchar(100) NOT NULL, 
"mark" varchar(256) NOT NULL, 
"profile_id" integer NULL REFERENCES "profile" ("id") DEFERRABLE INITIALLY DEFERRED, 
"date" datetime NULL, "dictation_difficulty" integer NOT NULL, 
"dictation_short_title" varchar(512) NOT NULL, 
"event_id" integer NULL REFERENCES "event" ("id") DEFERRABLE INITIALLY DEFERRED, 
"creator_id" integer NULL REFERENCES "profile" ("id") DEFERRABLE INITIALLY DEFERRED, 
"space_id" integer NULL REFERENCES "space" ("id") DEFERRABLE INITIALLY DEFERRED);
 

CREATE INDEX "auth_group_permissions_group_id_b120cbf9" ON "auth_group_permissions" ("group_id");
CREATE UNIQUE INDEX "auth_group_permissions_group_id_permission_id_0cd325b0_uniq" ON "auth_group_permissions" ("group_id", "permission_id");
CREATE INDEX "auth_group_permissions_permission_id_84c5c92e" ON "auth_group_permissions" ("permission_id");
CREATE INDEX "auth_permission_content_type_id_2f476e4b" ON "auth_permission" ("content_type_id");
CREATE UNIQUE INDEX "auth_permission_content_type_id_codename_01ab375a_uniq" ON "auth_permission" ("content_type_id", "codename");
CREATE INDEX "auth_user_groups_group_id_97559544" ON "auth_user_groups" ("group_id");
CREATE INDEX "auth_user_groups_user_id_6a12ed8b" ON "auth_user_groups" ("user_id");
CREATE UNIQUE INDEX "auth_user_groups_user_id_group_id_94350c0c_uniq" ON "auth_user_groups" ("user_id", "group_id");
CREATE INDEX "auth_user_user_permissions_permission_id_1fbb5f2c" ON "auth_user_user_permissions" ("permission_id");
CREATE INDEX "auth_user_user_permissions_user_id_a95ead1b" ON "auth_user_user_permissions" ("user_id");
CREATE UNIQUE INDEX "auth_user_user_permissions_user_id_permission_id_14a6b632_uniq" ON "auth_user_user_permissions" ("user_id", "permission_id");
CREATE INDEX "created_profile_by_user_creator_id_a4b8645d" ON "created_profile_by_user" ("creator_id");
CREATE INDEX "dictation_event_id_e26eb9d3" ON "dictation" ("event_id");
CREATE INDEX "dictation_space_id_574282ac" ON "dictation" ("space_id");
CREATE INDEX "django_admin_log_content_type_id_c4bce8eb" ON "django_admin_log" ("content_type_id");
CREATE INDEX "django_admin_log_user_id_c564eba6" ON "django_admin_log" ("user_id");
CREATE UNIQUE INDEX "django_content_type_app_label_model_76bd3d3b_uniq" ON "django_content_type" ("app_label", "model");
CREATE INDEX "django_session_expire_date_a5c62663" ON "django_session" ("expire_date");
CREATE INDEX "event_creator_id_a3f79688" ON "event" ("creator_id");
CREATE INDEX "main_team_profile_id_2afa6b98" ON "main_team" ("profile_id");
CREATE INDEX "news_author_id_064efc6e" ON "news" ("author_id");
CREATE INDEX "news_region_id_b418dd9e" ON "news" ("region_id");
CREATE INDEX "profile_user_id_2aeb6f6b" ON "profile" ("user_id");
CREATE INDEX "region_partner_region_id_105691f1" ON "region_partner" ("region_id");
CREATE INDEX "request_to_dictation_creator_id_8e212b37" ON "request_to_dictation" ("creator_id");
CREATE INDEX "request_to_dictation_dictation_id_31c28b38" ON "request_to_dictation" ("dictation_id");
CREATE INDEX "request_to_dictation_profile_id_c8923b6b" ON "request_to_dictation" ("profile_id");
CREATE INDEX "space_in_region_region_id_d95de199" ON "space_in_region" ("region_id");
CREATE INDEX "space_in_region_space_id_5265fc63" ON "space_in_region" ("space_id");
CREATE INDEX "team_of_space_space_id_963c1cbd" ON "team_of_space" ("space_id");
CREATE INDEX "team_of_space_teammate_id_a8c57c42" ON "team_of_space" ("teammate_id");
CREATE INDEX "user_in_region_profile_id_d94238d1" ON "user_in_region" ("profile_id");
CREATE INDEX "user_in_region_region_id_dbd1f761" ON "user_in_region" ("region_id");
CREATE INDEX "work_for_dictation_creator_id_09a564c2" ON "work_for_dictation" ("creator_id");
CREATE INDEX "work_for_dictation_event_id_9fa3a9ca" ON "work_for_dictation" ("event_id");
CREATE INDEX "work_for_dictation_profile_id_8defe04b" ON "work_for_dictation" ("profile_id");
CREATE INDEX "work_for_dictation_space_id_3fbfa528" ON "work_for_dictation" ("space_id");
 

```
