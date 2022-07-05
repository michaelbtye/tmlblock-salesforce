# TML Blocks - Salesforce

This document is intended to serve as a guide for a technical resource to implement the ThoughtSpot Starter Kit for Salesforce on any ThoughtSpot instance. The data set must be stored in a Snowflake data warehouse and accessed via Embrace from the ThoughtSpot instance. Because every customer’s Salesforce data will likely be somewhat different, the Starter Kit must be adjusted to match their data/definitions and desired customer use cases.


## Implementation Steps 

### Salesforce Standard Tables DDL
This section includes the DDL for the standard Salesforce tables used in the ThoughtSpot Starter Kit for Salesforce. A data extract from Salesforce containing only “out-of-the-box” fields can be loaded into these tables. These four tables (ACCOUNT, OPPORTUNITY, OPPORTUNITYHISTORY and USER) are the only Salesforce table required for the  ThoughtSpot Starter Kit for Salesforce.

Notes:
If customers express a desire to use custom fields, the user should adjust the DDL for these tables as necessary.
Foreign Key relationships have been removed from the DDL as they are not relevant in this implementation.

**ACCOUNT**
This table stores the data from the Salesforce Account data object.

```
CREATE OR REPLACE TABLE ACCOUNT (
	"Id" VARCHAR(18) NOT NULL,
	"IsDeleted" VARCHAR(1),
	"MasterRecordId" VARCHAR(18),
	"Name" VARCHAR(255),
	"Type" VARCHAR(40),
	"RecordTypeId" VARCHAR(18),
	"BillingStreet" VARCHAR(255),
	"BillingCity" VARCHAR(40),
	"BillingState" VARCHAR(80),
	"BillingPostalCode" VARCHAR(20),
	"BillingCountry" VARCHAR(80),
	"BillingLatitude" FLOAT,
	"BillingLongitude" FLOAT,
	"BillingGeocodeAccuracy" VARCHAR(40),
	"ShippingStreet" VARCHAR(255),
	"ShippingCity" VARCHAR(40),
	"ShippingState" VARCHAR(80),
	"ShippingPostalCode" VARCHAR(20),
	"ShippingCountry" VARCHAR(80),
	"ShippingLatitude" FLOAT,
	"ShippingLongitude" FLOAT,
	"ShippingGeocodeAccuracy" VARCHAR(40),
	"Fax" VARCHAR(40),
	"Website" VARCHAR(255),
	"PhotoUrl" VARCHAR(255),
	"Description" VARCHAR(32000),
	"CurrencyIsoCode" VARCHAR(3),
	"OwnerId" VARCHAR(18),
	"CreatedDate" TIMESTAMP_NTZ(9),
	"CreatedById" VARCHAR(18),
	"LastModifiedDate" TIMESTAMP_NTZ(9),
	"LastModifiedById" VARCHAR(18),
	"SystemModstamp" TIMESTAMP_NTZ(9),
	"LastActivityDate" DATE,
	"LastViewedDate" TIMESTAMP_NTZ(9),
	"LastReferencedDate" TIMESTAMP_NTZ(9),
	"IsPartner" VARCHAR(1),
	"IsCustomerPortal" VARCHAR(1),
	"JigsawCompanyId" VARCHAR(20),
	primary key ("Id")
);

```

**OPPORTUNITY**
This table stores the data from the Salesforce Opportunity data object.


```
CREATE OR REPLACE TABLE OPPORTUNITY (
	"Id" VARCHAR(18) NOT NULL,
	"IsDeleted" VARCHAR(1),
	"AccountId" VARCHAR(18),
	"RecordTypeId" VARCHAR(18),
	"Name" VARCHAR(120),
	"Description" VARCHAR(32000),
	"StageName" VARCHAR(255),
	"ExpectedRevenue" FLOAT,
	"CloseDate" DATE,
	"LeadSource" VARCHAR(40),
	"IsClosed" VARCHAR(1),
	"IsWon" VARCHAR(1),
	"ForecastCategory" VARCHAR(40),
	"ForecastCategoryName" VARCHAR(40),
	"CurrencyIsoCode" VARCHAR(3),
	"HasOpportunityLineItem" VARCHAR(1),
	"Pricebook2Id" VARCHAR(18),
	"OwnerId" VARCHAR(18),
	"CreatedDate" TIMESTAMP_NTZ(9),
	"CreatedById" VARCHAR(18),
	"LastModifiedDate" TIMESTAMP_NTZ(9),
	"LastModifiedById" VARCHAR(18),
	"SystemModstamp" TIMESTAMP_NTZ(9),
	"LastActivityDate" DATE,
	"FiscalQuarter" NUMBER(38,0),
	"FiscalYear" NUMBER(38,0),
	"Fiscal" VARCHAR(6),
	"LastViewedDate" TIMESTAMP_NTZ(9),
	"LastReferencedDate" TIMESTAMP_NTZ(9),
	"PartnerAccountId" VARCHAR(18),
	"SyncedQuoteId" VARCHAR(18),
	"ContractId" VARCHAR(18),
	"HasOpenActivity" VARCHAR(1),
	"HasOverdueTask" VARCHAR(1),
	primary key ("Id")
);

```

**OPPORTUNITYHISTORY**
This table stores the data from the Salesforce OpportunityHistory data object. Of all the tables, this one most closely resembles a fact table and includes all Opportunity-related actions that occur over the time span of the data set.


```
CREATE OR REPLACE TABLE OPPORTUNITYHISTORY (
	"Id" VARCHAR(18) NOT NULL,
	"OpportunityId" VARCHAR(18),
	"CreatedById" VARCHAR(18),
	"CreatedDate" TIMESTAMP_NTZ(9),
	"StageName" VARCHAR(255),
	"Amount" FLOAT,
	"ExpectedRevenue" FLOAT,
	"CloseDate" DATE,
	"Probability" NUMBER(38,0),
	"ForecastCategory" VARCHAR(40),
	"CurrencyIsoCode" VARCHAR(3),
	"SysModstamp" TIMESTAMP_NTZ(9),
	"IsDeleted" VARCHAR(1),
	primary key ("Id")
);

```

**USER**
This table stores the data from the Salesforce User data object.


```
CREATE OR REPLACE TABLE USER (
	"Id" VARCHAR(18) NOT NULL,
	"Username" VARCHAR(80),
	"LastName" VARCHAR(80),
	"FirstName" VARCHAR(40),
	"Name" VARCHAR(121),
	"CompanyName" VARCHAR(80),
	"Division" VARCHAR(80),
	"Department" VARCHAR(80),
	"Title" VARCHAR(80),
	"Street" VARCHAR(255),
	"City" VARCHAR(40),
	"State" VARCHAR(80),
	"PostalCode" VARCHAR(20),
	"Country" VARCHAR(80),
	"Latitude" FLOAT,
	"Longitude" FLOAT,
	"GeocodeAccuracy" VARCHAR(40),
	"Email" VARCHAR(128),
	"EmailPreferencesAutoBcc" VARCHAR(1),
	"EmailPreferencesAutoBccStayInTouch" VARCHAR(1),
	"EmailPreferencesStayInTouchReminder" VARCHAR(1),
	"SenderEmail" VARCHAR(80),
	"SenderName" VARCHAR(80),
	"Signature" VARCHAR(1333),
	"StayInTouchSubject" VARCHAR(80),
	"StayInTouchSignature" VARCHAR(512),
	"StayInTouchNote" VARCHAR(512),
	"Phone" VARCHAR(40),
	"Fax" VARCHAR(40),
	"MobilePhone" VARCHAR(40),
	"Alias" VARCHAR(8),
	"CommunityNickname" VARCHAR(40),
	"BadgeText" VARCHAR(80),
	"IsActive" VARCHAR(1),
	"TimeZoneSidKey" VARCHAR(40),
	"UserRoleId" VARCHAR(18),
	"LocaleSidKey" VARCHAR(40),
	"ReceivesInfoEmails" VARCHAR(1),
	"ReceivesAdminInfoEmails" VARCHAR(1),
	"EmailEncodingKey" VARCHAR(40),
	"DefaultCurrencyIsoCode" VARCHAR(3),
	"CurrencyIsoCode" VARCHAR(3),
	"UserType" VARCHAR(40),
	"LanguageLocaleKey" VARCHAR(40),
	"EmployeeNumber" VARCHAR(20),
	"DelegatedApproverId" VARCHAR(18),
	"ManagerId" VARCHAR(18),
	"LastLoginDate" TIMESTAMP_NTZ(9),
	"CreatedDate" TIMESTAMP_NTZ(9),
	"CreatedById" VARCHAR(18),
	"LastModifiedDate" TIMESTAMP_NTZ(9),
	"LastModifiedById" VARCHAR(18),
	"SystemModstamp" TIMESTAMP_NTZ(9),
	"OfflineTrialExpirationDate" TIMESTAMP_NTZ(9),
	"OfflinePdaTrialExpirationDate" TIMESTAMP_NTZ(9),
	"UserPermissionsMarketingUser" VARCHAR(1),
	"UserPermissionsOfflineUser" VARCHAR(1),
	"UserPermissionsCallCenterAutoLogin" VARCHAR(1),
	"UserPermissionsMobileUser" VARCHAR(1),
	"UserPermissionsSFContentUser" VARCHAR(1),
	"UserPermissionsKnowledgeUser" VARCHAR(1),
	"UserPermissionsInteractionUser" VARCHAR(1),
	"UserPermissionsSupportUser" VARCHAR(1),
	"UserPermissionsChatterAnswersUser" VARCHAR(1),
	"ForecastEnabled" VARCHAR(1),
	"UserPreferencesActivityRemindersPopup" VARCHAR(1),
	"UserPreferencesEventRemindersCheckboxDefault" VARCHAR(1),
	"UserPreferencesTaskRemindersCheckboxDefault" VARCHAR(1),
	"UserPreferencesReminderSoundOff" VARCHAR(1),
	"UserPreferencesDisableAllFeedsEmail" VARCHAR(1),
	"UserPreferencesDisableFollowersEmail" VARCHAR(1),
	"UserPreferencesDisableProfilePostEmail" VARCHAR(1),
	"UserPreferencesDisableChangeCommentEmail" VARCHAR(1),
	"UserPreferencesDisableLaterCommentEmail" VARCHAR(1),
	"UserPreferencesDisProfPostCommentEmail" VARCHAR(1),
	"UserPreferencesContentNoEmail" VARCHAR(1),
	"UserPreferencesContentEmailAsAndWhen" VARCHAR(1),
	"UserPreferencesHideCSNGetChatterMobileTask" VARCHAR(1),
	"UserPreferencesDisableMentionsPostEmail" VARCHAR(1),
	"UserPreferencesDisMentionsCommentEmail" VARCHAR(1),
	"UserPreferencesHideCSNDesktopTask" VARCHAR(1),
	"UserPreferencesHideChatterOnboardingSplash" VARCHAR(1),
	"UserPreferencesHideSecondChatterOnboardingSplash" VARCHAR(1),
	"UserPreferencesDisCommentAfterLikeEmail" VARCHAR(1),
	"UserPreferencesDisableLikeEmail" VARCHAR(1),
	"UserPreferencesSortFeedByComment" VARCHAR(1),
	"UserPreferencesDisableMessageEmail" VARCHAR(1),
	"UserPreferencesDisableBookmarkEmail" VARCHAR(1),
	"UserPreferencesDisableSharePostEmail" VARCHAR(1),
	"UserPreferencesEnableAutoSubForFeeds" VARCHAR(1),
	"UserPreferencesDisableFileShareNotificationsForApi" VARCHAR(1),
	"UserPreferencesShowTitleToExternalUsers" VARCHAR(1),
	"UserPreferencesShowManagerToExternalUsers" VARCHAR(1),
	"UserPreferencesShowEmailToExternalUsers" VARCHAR(1),
	"UserPreferencesShowWorkPhoneToExternalUsers" VARCHAR(1),
	"UserPreferencesShowMobilePhoneToExternalUsers" VARCHAR(1),
	"UserPreferencesShowFaxToExternalUsers" VARCHAR(1),
	"UserPreferencesShowStreetAddressToExternalUsers" VARCHAR(1),
	"UserPreferencesShowCityToExternalUsers" VARCHAR(1),
	"UserPreferencesShowStateToExternalUsers" VARCHAR(1),
	"UserPreferencesShowPostalCodeToExternalUsers" VARCHAR(1),
	"UserPreferencesShowCountryToExternalUsers" VARCHAR(1),
	"UserPreferencesShowProfilePicToGuestUsers" VARCHAR(1),
	"UserPreferencesShowTitleToGuestUsers" VARCHAR(1),
	"UserPreferencesShowCityToGuestUsers" VARCHAR(1),
	"UserPreferencesShowStateToGuestUsers" VARCHAR(1),
	"UserPreferencesShowPostalCodeToGuestUsers" VARCHAR(1),
	"UserPreferencesShowCountryToGuestUsers" VARCHAR(1),
	"UserPreferencesHideS1BrowserUI" VARCHAR(1),
	"UserPreferencesDisableEndorsementEmail" VARCHAR(1),
	"UserPreferencesPathAssistantCollapsed" VARCHAR(1),
	"UserPreferencesCacheDiagnostics" VARCHAR(1),
	"UserPreferencesShowEmailToGuestUsers" VARCHAR(1),
	"UserPreferencesShowManagerToGuestUsers" VARCHAR(1),
	"UserPreferencesShowWorkPhoneToGuestUsers" VARCHAR(1),
	"UserPreferencesShowMobilePhoneToGuestUsers" VARCHAR(1),
	"UserPreferencesShowFaxToGuestUsers" VARCHAR(1),
	"UserPreferencesShowStreetAddressToGuestUsers" VARCHAR(1),
	"UserPreferencesLightningExperiencePreferred" VARCHAR(1),
	"UserPreferencesHideEndUserOnboardingAssistantModal" VARCHAR(1),
	"UserPreferencesHideLightningMigrationModal" VARCHAR(1),
	"UserPreferencesHideSfxWelcomeMat" VARCHAR(1),
	"UserPreferencesHideBiggerPhotoCallout" VARCHAR(1),
	"UserPreferencesGlobalNavBarWTShown" VARCHAR(1),
	"UserPreferencesGlobalNavGridMenuWTShown" VARCHAR(1),
	"UserPreferencesCreateLEXAppsWTShown" VARCHAR(1),
	"UserPreferencesFavoritesWTShown" VARCHAR(1),
	"UserPreferencesRecordHomeSectionCollapseWTShown" VARCHAR(1),
	"UserPreferencesRecordHomeReservedWTShown" VARCHAR(1),
	"UserPreferencesFavoritesShowTopFavorites" VARCHAR(1),
	"ContactId" VARCHAR(18),
	"AccountId" VARCHAR(18),
	"CallCenterId" VARCHAR(18),
	"Extension" VARCHAR(40),
	"PortalRole" VARCHAR(40),
	"IsPortalEnabled" VARCHAR(1),
	"FederationIdentifier" VARCHAR(512),
	"AboutMe" VARCHAR(1000),
	"FullPhotoUrl" VARCHAR(1024),
	"SmallPhotoUrl" VARCHAR(1024),
	"IsExtIndicatorVisible" VARCHAR(1),
	"MediumPhotoUrl" VARCHAR(1024),
	"DigestFrequency" VARCHAR(40),
	"DefaultGroupNotificationFrequency" VARCHAR(40),
	"LastViewedDate" TIMESTAMP_NTZ(9),
	"LastReferencedDate" TIMESTAMP_NTZ(9),
	"BannerPhotoUrl" VARCHAR(1024),
	"SmallBannerPhotoUrl" VARCHAR(1024),
	"MediumBannerPhotoUrl" VARCHAR(1024),
	"IsProfilePhotoActive" VARCHAR(1),
	primary key ("Id")
);

```

### Salesforce Starter Kit Views + Tables DDL
This section includes the DDL for the views that are used to “feed data” to ThoughtSpot. When the Embrace Connection is created to Snowflake, these views are the data objects that are selected by the user.

Notes:
If custom fields are used in the underlying data tables, the user must adjust the DDL for these views as necessary.
These views were created as part of an implementation using (generated) ThoughtSpot Salesforce data. Items such as stage names, regions, use cases or business departments will need to be adjusted in order to work properly with customer data. (And some may not work with customer data at all). All view syntax that is likely to require alteration has been highlighted in bolded.

**ACCOUNT**
This view selects the subset of fields from the ACCOUNT table required for the ThoughtSpot Starter Kit for Salesforce.


```
CREATE OR REPLACE VIEW ACCOUNT
AS
SELECT "Id" AS "AccountId",
"Name",
"Type",
"BillingCountry",
"Website",
"Description",
"IsPartner"
FROM salesforce."ACCOUNT";

```

**OPPORTUNITY**

This view selects the subset of fields from the OPPORTUNITY table required for the ThoughtSpot Starter Kit for Salesforce.

The yellow highlighting indicates the syntax that will need to be customized for every customer based upon:
Salesforce stage definitions.
Whether Opportunity names or other data in the OPPORTUNITY table contains Department and/or Use Case information.


```

CREATE OR REPLACE VIEW OPPORTUNITY
AS
SELECT o."Id" AS "OpportunityId",
o."AccountId",
o."Name",
o."StageName",
o."ExpectedRevenue",
o."CloseDate",
o."LeadSource",
o."CurrencyIsoCode",
o."OwnerId",
o."CreatedDate",
o."LastActivityDate",
o."FiscalQuarter",
o."FiscalYear",
o."Fiscal",
o."PartnerAccountId",
**CASE WHEN o."StageName" = 'closed dq/lost' THEN o."ExpectedRevenue" ELSE 0 END AS lost_pipeline,
CASE WHEN o."StageName" = 's6 closed won' THEN o."ExpectedRevenue" ELSE 0 END AS amount_booked,
CASE WHEN o."StageName" != 'closed dq/lost' AND o."StageName" != 's6 closed won' THEN o."ExpectedRevenue" ELSE 0 END AS open_pipeline,
CASE WHEN o."StageName" = 'closed dq/lost' THEN 'Lost' WHEN o."StageName" = 's6 closed won' then 'Booked' ELSE 'Open' END AS status,
SUBSTR(o."Name", 0, POSITION(' - ', o."Name") - 1) AS "Department",
SUBSTR(o."Name", POSITION('-', o."Name") + 2, (POSITION(' (', o."Name") - 2) - POSITION('-', o."Name")) AS "UseCase"**
FROM PMMDB.salesforce."OPPORTUNITY" o;

```

**OPPORTUNITYSTATS**
This view contains primarily derived fields resulting from CASE statements and AGGREGATIONS using the OPPORTUNITY and OPPORTUNITYHISTORY table. The yellow highlighting over the entire view indicates that the syntax will need to be customized for every customer based upon their Salesforce stage definitions.

(This view uses the ThoughtSpot definitions which include 2 marketing lead designations and 6 designations for sales accepted leads).


```
CREATE OR REPLACE VIEW OPPORTUNITYSTATS
AS
SELECT iv."OpportunityId",
iv.m0_m1,
iv.m1_s1,
iv.s1_s2,
iv.s2_s3,
iv.s3_s4,
iv.s4_s5,
iv.s5_s6,
iv.m0_dq,
iv.m1_dq,
iv.s1_dq,
iv.s2_dq,
iv.s3_dq,
iv.s4_dq,
iv.s5_dq,
CASE WHEN iv.m0_dq IS NOT NULL THEN 'm0 initial meeting set'
	WHEN iv.m1_dq IS NOT NULL THEN 'm1 new business meeting'
	WHEN iv.s1_dq IS NOT NULL THEN 's1 business exposure'
	WHEN iv.s2_dq IS NOT NULL THEN 's2 scoping success'
	WHEN iv.s3_dq IS NOT NULL THEN 's3 alignment with eb'
	WHEN iv.s4_dq IS NOT NULL THEN 's4 validation'
	WHEN iv.s5_dq IS NOT NULL THEN 's5 negotiate'
    WHEN iv.s5_s6 IS NOT NULL THEN 'won'
    ELSE 'live' END AS "FinalStageBeforeDQ",
iv."Amount" AS "Amount",
IFNULL(iv2."WinOrder", 0) AS "WinOrder",
iv2."WinTimeSpan"
FROM (
	SELECT iv."OpportunityId",
    MAX(iv."Amount") AS "Amount",
		MAX(CASE WHEN iv."StageName" = 'm0 initial meeting set' AND iv.nextstage = 'm1 new business meeting' THEN daysbetween ELSE NULL END) AS m0_m1,
		MAX(CASE WHEN iv."StageName" = 'm1 new business meeting' AND iv.nextstage = 's1 business exposure' THEN daysbetween ELSE NULL END) AS m1_s1,
		MAX(CASE WHEN iv."StageName" = 's1 business exposure' AND iv.nextstage = 's2 scoping success' THEN daysbetween ELSE NULL END) AS s1_s2,
		MAX(CASE WHEN iv."StageName" = 's2 scoping success' AND iv.nextstage = 's3 alignment with eb' THEN daysbetween ELSE NULL END) AS s2_s3,
		MAX(CASE WHEN iv."StageName" = 's3 alignment with eb' AND iv.nextstage = 's4 validation' THEN daysbetween ELSE NULL END) AS s3_s4,
		MAX(CASE WHEN iv."StageName" = 's4 validation' AND iv.nextstage = 's5 negotiate' THEN daysbetween ELSE NULL END) AS s4_s5,
		MAX(CASE WHEN iv."StageName" = 's5 negotiate' AND iv.nextstage = 's6 closed won' THEN daysbetween ELSE NULL END) AS s5_s6,
		MAX(CASE WHEN iv."StageName" = 'm0 initial meeting set' AND iv.nextstage = 'closed dq/lost' THEN daysbetween ELSE NULL END) AS m0_dq,
		MAX(CASE WHEN iv."StageName" = 'm1 new business meeting' AND iv.nextstage = 'closed dq/lost' THEN daysbetween ELSE NULL END) AS m1_dq,
		MAX(CASE WHEN iv."StageName" = 's1 business exposure' AND iv.nextstage = 'closed dq/lost' THEN daysbetween ELSE NULL END) AS s1_dq,
		MAX(CASE WHEN iv."StageName" = 's2 scoping success' AND iv.nextstage = 'closed dq/lost' THEN daysbetween ELSE NULL END) AS s2_dq,
		MAX(CASE WHEN iv."StageName" = 's3 alignment with eb' AND iv.nextstage = 'closed dq/lost' THEN daysbetween ELSE NULL END) AS s3_dq,
		MAX(CASE WHEN iv."StageName" = 's4 validation' AND iv.nextstage = 'closed dq/lost' THEN daysbetween ELSE NULL END) AS s4_dq,
		MAX(CASE WHEN iv."StageName" = 's5 negotiate' AND iv.nextstage = 'closed dq/lost' THEN daysbetween ELSE NULL END) AS s5_dq
	FROM (
		SELECT iv."OpportunityId",
		iv."StageName",
		iv."CreatedDate",
        iv."Amount",
		LEAD(iv."StageName", 1) OVER (PARTITION BY iv."OpportunityId" ORDER BY iv."SysModstamp") AS nextstage,
		DATEDIFF('day', iv."CreatedDate", LEAD(iv."CreatedDate", 1) OVER (PARTITION BY iv."OpportunityId" ORDER BY iv."SysModstamp")) AS daysbetween
		FROM (
			SELECT h."OpportunityId",
			h."StageName",
			MIN(h."CreatedDate") AS "CreatedDate",
            MIN(h."SysModstamp") AS "SysModstamp",
            MAX(h."Amount") AS "Amount",
			COUNT(*) AS cnt
			FROM PMMDB.salesforce.OPPORTUNITYHISTORY h
			INNER JOIN
			PMMDB.salesforce.OPPORTUNITY o
			ON h."OpportunityId" = o."Id"
			GROUP BY 1, 2
		) iv
	) iv
	GROUP BY 1
) iv
LEFT OUTER JOIN
(
  SELECT o."Id" AS "OpportunityId",
  o."AccountId",
  o."CloseDate",
  DATEDIFF(day, o."CreatedDate", o."CloseDate") AS "WinTimeSpan",
  ROW_NUMBER() OVER (PARTITION BY o."AccountId" ORDER BY o."CloseDate") AS "WinOrder"
  FROM PMMDB.salesforce.OPPORTUNITY o
  WHERE o."IsWon" = 't'
) iv2
ON iv."OpportunityId" = iv2."OpportunityId";


```


