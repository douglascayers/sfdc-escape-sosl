Apex Escape SOSL
================

Apex utility class for escaping text input for the [FIND {SearchQuery}](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_sosl_find.htm#topic-title) part of [Dynamic SOSL queries](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_dynamic_sosl.htm).

* Removes reserved logical operators: `AND NOT`, `AND`, `OR`
* Escapes special characters: `? & | ! { } [ ] ( ) ^ ~ * : " ' + -`
* Returns null if text is blank


Installation
------------
You can easily install these components to your org straight from github. **ProTip:** Test in a sandbox or developer org before production.
* [Deploy from Github](https://githubsfdeploy.herokuapp.com)


Usage
-----

    // test data with special characters
    Account acct = new Account( name = '[salesforce] foo\'bar' );
    insert acct;

    // escape your search term
    String text = '[salesforce] foo\'bar';               //--> "[salesforce] foo'bar"
    String escapedText = SearchUtils.escapeSosl( text ); //--> "\\[salesforce\\] foo\'bar"

    // dynamic sosl query
    String query =
        ' FIND \'' + escapedText + '\'' +
        ' RETURNING Account ( id, name ) ' +
        ' LIMIT 5 '
    ;
    System.debug( query );

    // execute query
    List<List<SObject>> results = Search.query( query );
    System.debug( results );

