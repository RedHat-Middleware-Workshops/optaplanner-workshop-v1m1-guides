
# OptaPlanner Introduction

In this section you will learn:

1. How to use your Business Object Model to automate decisions and policies.

2. The different types of authoring tools available to you.

3. How to use your automated decisions and rules.


## What is a Planning Problem?


## The Authoring Tools


### Decision Tables

A very common way to define the logic behind risk assessment is to store this information in spreadsheets. With Red Hat Process Automation Manager you can use the same spreadsheet approach and make it an executable asset (i.e. a set of rules) in the engine. In this section we are going to create a _Decision Table_ to automate the risk assessment rules that were given to you.

1. First we go back to the Library view and we click on the blue `Add Asset` button in the top right corner.

    ![Business Central Decision Table Add Asset]({% image_path business-central-decision-table-add-asset.png %}){:width="600px"}

2. We select `Guided Decision Table` from the catalog of assets (the UI allows you to filter the assets per type by using the filter drop-down and input box in the upper-left of the screen. Select `Decisions` to filter on decision assets).

    ![Business Central Decision Table Add Asset Guided]({% image_path business-central-decision-table-add-asset-guided.png %}){:width="600px"}

3. Type the following values on the `Create New Decision Table` wizard

    - Guided Decision Table (Name): `risk-evaluation`
    - Package: `com.myspace.ccd_project`

    Click _Ok_ and _Finish_.

4. You should see the `Guided Decision Table` editor with an empty table.

    ![Business Central Decision Table New]({% image_path business-central-decision-table-new.png %}){:width="600px"}

    There are 5 tabs in the editor:

    - _Model_: The model diagram of the Decision Table
    - _Columns_: Add, Edit or Delete columns in your table. A column can represent an Attribute, Metadata, a Constraint (rule left-hand-side or LHS) on a property of a Business Model Object, or an Action (rule right-hand-side or RHS).
    - _Overview_: Contains the meta-information of your asset: Version, Description, Last Modified, etc.
    - _Source_: Is the actual source code that is generated from the Decision Table Model. In the runtime engine, decision tables are translated into native DRL (Drools Rule Language), where each row in the table is translated into a rule.
    - _Data Objects_: Lists the Business Objects available to the editor to be used as conditions and/or actions.

    In our system, the properties evaluated to determine the risk scoring are:

    - Status of the Credit Card Holder
    - Total Amount disputed from the Fraud Data

    Let's add the Credit Card Holder condition column

5. Go to the _Columns_ tab and click on the button `Insert Column`, select `Add Condition` and click Next.

    ![Business Central Add Condition]({% image_path business-central-add-condition.png %}){:width="600px"}

6. We need to define which object is going to be evaluated. Click on `Create new Fact Pattern`. Select `CreditCardHolder` as the _Fact type_ and define a variable called `holder` as the _Binding_. Click _Next_.

    ![Business Central Create Pattern]({% image_path business-central-create-pattern.png %}){:width="600px"}

7. The calculation type is the type of evaluation that we are going to apply. In this case it will be against literal values. Select `Literal value` and click _Next_.

8. We want to define a constraint on the card holder's status, so we select the `status` field and click _Next_.

    ![Business Central Create Pattern Field]({% image_path business-central-create-pattern-field.png %}){:width="600px"}

9. Next we select the operator for the constraint. Select `equal to` from the drop down menu and click _Next_.

    ![Business Central Create Pattern Field Operator]({% image_path business-central-create-pattern-field-operator.png %}){:width="600px"}

10. Since there are only 3 possible status, we are going to configure the _Value list_ with the following values:

    `Standard,Silver,Gold`

    Set the _Default value_ to `Standard` and then click Next.

    ![Business Central Create Pattern Field Values]({% image_path business-central-create-pattern-field-values.png %}){:width="600px"}

11. We can now configure the header of the column

    Header: `Status`

    Click Finish and go back to the `Model` tab in the editor. You should see the newly created column.

    ![Business Central Create Pattern Field Header]({% image_path business-central-create-pattern-field-header.png %}){:width="600px"}

12. Repeat the same steps to add 2 more columns:
    - Pattern:
        - Fact type: `FraudData`
        - Binding: `data`
    - Calculation Type: `Literal value`
    - property: `totalFraudAmount`
    - operation:
        - `greater than` for one column. Use header name: "Minimum Amount"
        - `less than or equal to` for the second column. Use header nam "Maximum Amount"

    Note that for the second column you don't need to create a new fact pattern, you can reuse the existing one.

    At the end your decision table should look like this:

    ![Business Central Decision Table Columns]({% image_path business-central-decision-table-columns.png %}){:width="600px"}

13. Click on the _Save_ button to save the decision table.

14. Next go to the `Columns` tab and Click on `Insert Column`. This time we are adding an Action, the Right-Hand-Side of a rule. This action will be fired when the conditions are met. Select `Set the value of a field` and click next.

15. We want to set the risk scoring property of the `FraudData` object. So in the dropdown menu select the object `FraudData` bound to the variable `data`.Click Next.

    ![Business Central Decision Table Columns Action Data]({% image_path business-central-decision-table-columns-action-data.png %}){:width="600px"}

16. Select the field `disputeRiskRating` and click Next. We don't have a list of values so click Next. Type `Risk Scoring` as the header for the column and click Finish.

    ![Business Central Decision Table Columns Action Data Finish]({% image_path business-central-decision-table-columns-action-data-finish.png %}){:width="600px"}

17. Click on the _Save_ button to save the decision table.

18. Go back to your `Model` tab, which should show the following decision table.

    ![Business Central Decision Table Columns Action Data Finish Model]({% image_path business-central-decision-table-columns-action-data-finish-model.png %}){:width="600px"}

    We are now going to add the actual constraints and actions, i.e. the actual rules. Looking at our requirements, the first constraint is defined as:

    _For a standard customer, and a dispute amount between 0 and 100, the risk is low._

    There are 4 levels of risk: low, medium, high and very-high. We will defined these risk-levels as integers: 1,2,3, and 4.

19. Click on the button Insert and select append row from the dropdown menu.

     ![Business Central Decision Table Columns Action Data Finish Model]({% image_path business-central-decision-table-append-row.png %}){:width="600px"}

20. Click on the _Description_ cell of the new row and type "_Standard costumer low risk_". Use the following values for the other columns:

     - Description:`Standard costumer low risk`{{copy}}  
     - Status:`Standard`{{copy}}  
     - Minimum Amount:`0`{{copy}}  
     - Max Amount:`100`{{copy}}  
     - Risk Scoring:`0`{{copy}}

    Your decision table should look like this. Click Save.

    ![Business Central Decision Table First Row]({% image_path business-central-decision-table-first-row.png %}){:width="600px"}

    Apply the same procedure for the rest of the rules. At the end your decision table should look as follows:

    ![Business Central Decision Table First Row]({% image_path business-central-decision-table-complete.png %}){:width="600px"}


## Guided Rules

Guided Rules are one of the various types of rules you can create in Business Central. Once you have defined the Business Object Model, you can create rules that check conditions on the properties of these objects, rules that define conditions on combinations of objects, etc. For example, you can define a rule with a constraint on a Credit Card Holder's age, his/her status, riskRating, etc. If the condition or conditions are met, the rule is set to be _matched_ and becomes eligible for _firing_. When the rule fires, it executes the action defined in the rule. The action is the _THEN_ part of the rule, or what is also called the rule's consequence, or Right-Hand-Side (RHS).

In the previous section we've created the rules, in the form a decision table, that determine the credit risk scoring. In this section, we want to create the rules that determine whether the credit card dispute is eligible for automatic chargeback. We will create this rule in the form of _Guided Rule_.

In the case of the rules for automatic chargeback we are evaluating only the Credit Card Holder. So let's create the rule.

First we need to tell the rule what object or collection of objects is going to be evaluated. Rules have a very basic syntax, and basically consist of 3 parts:

- the _When_ part defines the constraints of the rule. I.e. these are the discrimination criteria or conditions which, if they are met, cause the rule to fire.
- the _Then_ part defines the actions the rule will execute when it fires. This can be for example setting specific data on a fact (in the Business Object Model), but this can also be inserting new, inferred, data into the rules engine. For example, based on a card holder's age, we can infer that he/she is an adult.
- the _properties_ or _attributes_ part. Here we can set additional characteristics of the rule, for example the group of rules it belongs to.

To create the rule, you:

1. Select the project ccd-project in the space MySpace

    ![Business Central Asset CCD Project]({% image_path business-central-asset-ccd-project.png %}){:width="600px"}

2. Click on the blue button `Add Asset` on the right upper corner of the Library View.

    ![Business Central CCD BOM Project]({% image_path business-central-ccd-bom-project.png %}){:width="600px"}

3. Int the "Add Asset" screen, select "Decision" from the drop-down filter menu to filter on decision assets.

    ![Business Central Add Assets Filter]({% image_path business-central-add-assets-filter.png %}){:width="600px"}

4. Select `Guided Rule` from the filtered catalog of Wizards.

5. Set the following data in the creation wizard:

    - Name: `automated-chargeback`
    - Package: `com.myspace.ccd_project`

    ![Business Central Guided Rule New]({% image_path business-central-guided-rule-new.png %}){:width="600px"}

6. Click ok. You should see a banner in green telling you that the asset was success fully created. The UI will display the editor that allows you to author your rule.

    ![Business Central Guided Rule New Wizard]({% image_path business-central-guided-rule-new-wizard.png %}){:width="600px"}

7. You will see 4 tabs in the editor panel. Select the tab that says "Data Objects"

    ![Business Central Guided Rule Import Data Object]({% image_path business-central-guided-rule-import-data-object.png %}){:width="600px"}

8. You should see 4 items listed: `AdditionalInformation`, `CreditCardHolder`, `FraudData`, and `Number`. These are shown by default as the rule is created in the same folder/package as these data objects. If `CreditCardHolder` is not listed, click on the blue _New Item_ button to import it.

    ![Business Central Guided Rule Import Data Object New]({% image_path business-central-guided-rule-import-data-object-new.png %}){:width="600px"}

9. Return to the _Model_ tab and Click on the green plus-sign to the right of the word _WHEN_.

    ![Business Central Guided Rule New Fact]({% image_path business-central-guided-rule-new-fact.png %}){:width="600px"}

10. Select the object `CreditCardHolder`, and click ok. We are now telling the rule engine that every time there is a CreditCardHolder we will activate this rule.

    ![Business Central Guided Rule New Fact Select]({% image_path business-central-guided-rule-new-fact-select.png %}){:width="600px"}

    In order to match the criteria of the functional requirement, we need to add a restriction on one of the card holder's properties. Automated chargeback is only approved for CC Holders that have the `status` _Gold_ or _Platinum_.

11. Click on the condition `There is a Credit Card Holder`, a new wizard will open. We are now going to _Add a restriction on a field_. From the dropdown box select the `status` field of the CC Holder.

    ![Business Central Guided Rule New Property Select]({% image_path business-central-guided-rule-new-property-select.png %}){:width="600px"}

12. From the dropdown box we select that the status `is contained in the (comma separated) list`. Click on the pencil icon and add the literal values  _Gold_ and _Platinum_, separated by a comma. TIP: You can also add enumerations containing these values to have them pre-populated for you. Click on the _Save_ button to save the asset.

    ![Business Central Guided Rule New Property Select Values]({% image_path business-central-guided-rule-new-property-select-values.png %}){:width="600px"}

13. Go back to the _Data Objects_ tab. If the `FraudData` data object has not been imported yet, complete the same procedure, to import it. Go back to the _Model_ tab and add a constraint on the `FraudData` object the same way as we did before. We don't need to put a constraint on any property of the `FraudData`, we just need to make sure that it's there.

    ![Business Central Guided Rule Check Fraud Data]({% image_path business-central-guided-rule-check-fraud-data.png %}){:width="600px"}

14. When you want to modify the data in the objects of the Business Model or facts, you need to be able to reference the matched object from within the rule. To allow this, the object needs to be bound to a variable inside the rule. This makes the object accessible in both the left-hand-side (LHS) and  right-hand-side (RHS) through the variable. Click on the fact declaration `There is FraudData`, the wizard to modify the constraints will open.

15. In the _Variable name_ field at the bottom of the form, type `data` as the name of the variable that you want to bind the `FraudData` object to. Click on the _Set_ button. Save the asset.

    ![Business Central Guided Rule Modify Fraud Data]({% image_path business-central-guided-rule-modify-fraud-data.png %}){:width="600px"}

Now we are going to set the property of automated chargeback to true on the `FraudData` object, so the dispute can be processed accordingly. Since this is the decision we are making, and thus the _action_ of the rule, we will define this as the THEN clause,  also known as the Right Hand Side (RHS) or Action section of our rule.

All of the information of the CC dispute is stored in facts. These facts can live in a session that the engine will keep in memory. So every time you evaluate a new fact, or change something to an existing fact, you will have all of the Objects in the session available in the process of decision making. In the RHS, or action, part of the rule you can change the values of any property on the objects that you can reference via the variables, or even create and add new objects/facts to the session (this is usually referred to as _inferring_ new data or information). Every time a property in an object changes, all of the decisions in which this property is used will be reevaluated to make sure that no other rule needs to be applied.

1. Click on the green plus-sign next to the _THEN_ keyword. When the `Add new action` wizard opens select `Change field values of data` and click on _OK_. This will automatically select the `FraudData` object, as this is the only object we've bound to a variable.

    ![Business Central Guided Rule Modify Fraud Data Wizard]({% image_path business-central-guided-rule-modify-fraud-data-wizard.png %}){:width="600px"}

2. Now we are going to set the value of the property `automated` to `true`, indicating that an automatic chargeback applies. Click on  the action `Set value of FraudData [data]` and select the field `automated`. Click on the pencil icon to the right and assign a literal value to the property.

    ![Business Central Guided Rule Modify Fraud Automated]({% image_path business-central-guided-rule-modify-fraud-automated.png %}){:width="600px"}

3. select `true` as the value for the automated property (this is the default value for booleans, so the property is probably already set to `true`). Note that since the type of data is `boolean`, you can only choose between `true` and `false`.

    ![Business Central Guided Rule Modify Fraud Automated True]({% image_path business-central-guided-rule-modify-fraud-automated-true.png %}){:width="600px"}

4. To validate that everything is correct, click on the _Validate_ button on the right and you should see a green "Item successfully validated!" message. Next, click on _Save_ to save the rule.

    ![Business Central Guided Rule Validate]({% image_path business-central-guided-rule-validate.png %}){:width="600px"}

You have created your first Business Rule using the Guided editor

## Decision Model & Notation (DMN)

Red Hat Process Automation Manager 7 supports the Decision Model & Notation (DMN) v1.2 standard. This means that models created in the DMN v1.1 or v1.2 specification can be imported into, and executed on, RHPAM. Apart from using Red Hat Process Automation Manager's and Red Hat Decision Manager's DMN editor, this also allows users to create DMN models in third-party editors, for example Trisotech's Digital Enterprise Suite, and execute then in RHPAM. In the following image we can see some examples of the types of diagrams you can create to define, in this case, the rules to calculate risk.

![Business Central Trisotech DMN]({% image_path business-central-trisotech-dmn.png %}){:width="600px"}

DMN uses a language business friendly called FEEL or Friendly Enough Expression Language.

![Business Central DMN FEEL]({% image_path business-central-dmn-feel.png %}){:width="600px"}

DMN is out of scope for this workshop. However, the specification provides an additional, interesting, and standard way to model and execute decisions in your business applications.