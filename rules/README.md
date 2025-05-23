# Rules for the Power CAT Tools - Code Review Tool

This document provides a categorized list of patterns and recommendations for improving the quality, performance, and maintainability of your Power Platform solutions. The rules are grouped into sections for Power Automate, Power Apps, and Copilot Studio to help you quickly find relevant guidance.

## Power Apps Patterns

### Performance

#### 1. Unused Variables (app-UnusedVariables)
- **Severity**: Medium
- **Category**: Performance
- **Description**: This pattern refers to any variables, assets, or controls that has been defined but is not being or rarely used anywhere within the app.
- **Recommendation**: 
  - Use Direct Data Source Operations: Instead of storing data in collections, perform operations directly on data sources whenever possible. This reduces memory usage and improves app responsiveness. User for Named formulas is also strongly encouraged.
  - Use Context Variables Sparingly: Context variables are useful for passing values between screens, but overusing them can lead to complex and hard-to-maintain code. Opt for direct data binding and formulas where feasible.
  - Adopt Component-Based Design: Break down your app into reusable components. This modular approach can reduce the need for global variables and collections, as each component can manage its own state.

#### 2. Excessive Database or API Requests (app-CheckForNplusOneCalls)
- **Severity**: High
- **Category**: Performance
- **Description**: This pattern typically occurs using gallery control. When we trigger too many requests to servers as a result of each item in the gallery performing a lookup on a direct data source.
- **Recommendation**: Consider using a local collection for the lookup used within the gallery provided the data fetched is of small size. Alternatively, building an API server side to flatten the response is a preferred approach but requires pro-code development.

#### 3. Inefficient Data Retrieval (app-FormulaOptimization-FirstFilter)
- **Severity**: High
- **Category**: Performance
- **Description**: Using `Filter()` or `Search()` functions inside `First()` or `Last()` can be inefficient, especially with external data sources, as it requires processing the entire dataset in memory before selecting the first or last record. This can lead to performance issues, particularly with large datasets.
- **Recommendation**: Optimize data retrieval by avoiding inefficient use of functions. Instead of using `First(Filter(...))` or `Last(Search(...))`, Consider using `Lookup()` function to optimize performance.

#### 4. Nested API Calls (app-FormulaOptimization-NestedApiCalls)
- **Severity**: Medium
- **Category**: Performance
- **Description**: This pattern checks for nested API calls within Filter() and LookUp() operations which can lead to performance issues and make code readability harder. Making API calls inside loops or nested within other API calls can significantly degrade performance, leading to increased latency and potential throttling issues.

#### 5. Asset Optimization (app-AssetOptimization)
- **Severity**: Medium
- **Category**: Performance
- **Description**: Large asset sizes, such as high-resolution images, oversized media files, and uncompressed resources, can significantly impact app performance, increasing load times and affecting responsiveness, especially on mobile devices.
- **Recommendation**: Ensure that all files do not exceed 300KB file size. Please consider compressing the listed files using optimized file formats (e.g., SVG for icons, WebP for images), and limiting media file sizes.

#### 6. Patch Formula Optimization (app-FormulaOptimization-Patch)
- **Severity**: Low
- **Category**: Performance
- **Description**: Optimize Patch formulas by avoiding unnecessary LookUp() functions.
- **Recommendation**: Ensure the second parameter is not using a LookUp() function. It is a best practice to use ThisItem whenever patching from a gallery or { id: userid }.

#### 7. App Settings Flags (app-AppSettings)
- **Severity**: Medium
- **Category**: Performance
- **Description**: Ensure that all necessary settings are enabled for optimal performance.
- **Recommendation**: Please consider turning on the appropriate app settings.

#### 8. Nested Filters (app-FormulaOptimization-NestedFilter)
- **Severity**: Medium
- **Category**: Performance
- **Description**: Consider simplifying nested formulas with Search() and Filter() operations. Avoiding the nesting of these two together leads to a more compact/concise formula and performance improvement.
- **Recommendation**: 
  - Use a single `Filter()` call with multiple conditions when possible
  - Apply filtering before searching to reduce dataset size before running expensive operations
  - Optimize for delegation when working with external data sources to ensure efficient querying

#### 9. Cross Screen Dependencies (app-InefficientDelayLoading)
- **Severity**: Medium
- **Category**: Performance
- **Description**: Cross page references force the load of additional screens if one screen has a dependency on a control in a different screen.
- **Recommendation**: Keep cross screen dependencies between screens low. Use variable & collection rather than values from controls properties.

#### 10. Unused Media Resources (app-UnusedMediaResources)
- **Severity**: Low
- **Category**: Performance
- **Description**: The app contains media (.jpg, .jpeg, .gif, .png, .bmp, .tif, .tiff, .svg, .wav, .mp3, .mp4) that have been uploaded to the app but not used in any screen.
- **Recommendation**: You can remove all unused media from the app to clean up or reduce the size of the app using Media > Remove unused media option.

### Accessibility

#### 11. Accessibility (app-Accessibility)
- **Severity**: High
- **Category**: Accessibility
- **Description**: Accessibility ensures that all users, including those with disabilities, can interact with and use the PowerApps application effectively.
- **Recommendation**: Review accessibility checker to address issues related to screen reader compatibility, keyboard navigation, sufficient color contrast, and alternative text for images. Notes: Alternate Text labels can reveal otherwise hidden media and context to users with disabilities. Use natural language text when naming screens to optimize results from screen readers.

### Maintainability

#### 12. Named Formulas (app-NamedFormulas)
- **Severity**: Low
- **Category**: Maintainability
- **Description**: This pattern checks for the use of Named Formulas in your application.

#### 13. Code Readability (app-CodeReadability)
- **Severity**: Low
- **Category**: Maintainability
- **Description**: This patterns checks for long PowerFx formulas that are hard to read and spans multiple lines of code.
- **Recommendation**: 
  - Avoid expressions or formulas that are very long as they will become hard to read
  - Examples include, but are not limited to nested If/Then/Else statements and nested operators such as OR or AND
  - Avoid repetitive code
  - Use format text to enhance readability

### Functionality

#### 14. Error Handling (app-ErrorHandling)
- **Severity**: Low
- **Category**: Functionality
- **Description**: Proper error handling ensures that unexpected issues, such as failed data operations or invalid inputs, are managed gracefully, preventing a poor user experience. It is best to use the IfError() function to manage data validation for Patch() commands and the OnSuccess/OnFail properties when submitting form data.
- **Recommendation**: Use `IfError()` and `Notify()` functions to catch errors and provide user-friendly messages. Implement logging mechanisms to track failures and improve debugging.

### UX

#### 15. UX Layout (app-UxLayout)
- **Severity**: Low
- **Category**: UX
- **Description**: This patterns checks for use of layout containers in your app determine if you are app is responsive.
- **Recommendation**: Using layout containers can significantly enhance the design and functionality of your apps:
  - **Responsive Design**: Layout containers are essential for creating responsive designs that adapt to different screen sizes and orientations
  - **Organized Structure**: Containers help you organize your app's interface by grouping related controls together
  - **Improved Accessibility**: Containers aid screen reader users in understanding the structure of the app
  - **Enhanced Performance**: By reducing the number of individual layout calculations, containers can improve the performance of your app

## Power Automate Flow Patterns

### Performance

#### 16. Avoid Nested For-Each Loops (flow-NestedLoops)
- **Severity**: Medium
- **Category**: Performance
- **Description**: Nested "For Each" loops can be an expensive operation in Power Automate due to their potential impact on performance and resource consumption.
- **Recommendation**: A more efficient implementation is to use OData Query Expansion where we only need to work with a single Apply-for-Each loop. This also reduces the total requests against Dataverse to just one RetrieveMultiple call.

#### 17. Parallel Execution and Concurrency (flow-Concurrency)
- **Severity**: Medium
- **Category**: Performance
- **Description**: Power Automate supports parallel execution meaning flows can have two or more steps that run simultaneously, after which the workflow will only proceed once all parallel steps have completed.
- **Recommendation**: 
  - A good rule of thumb is to parallelize only when actions take more than 5 seconds to execute
  - Use parallel branches for sending non-blocking approval requests, creating "Quorum" based approvals, creating or updating records in multiple systems

#### 18. Static Variables/Unused Variables (flow-StaticVariables)
- **Severity**: Medium
- **Category**: Performance
- **Description**: Checks for variables which have been initialized but are never updated in the power automate flow using any of the variable actions. The values of the variables defined during initialization are referenced in the power automate. These variables can be replaced with Compose actions as they are executed faster and can be referenced.
- **Recommendation**: 
  - Compose are not update-able at run-time. It is useful for write-once, read-many type of usage
  - If we need to write or update in many places in the flow, variables will be much more appropriate
  - Using compose to create variables can also perform faster than initializing+ Declare variables

#### 19. Optimize Power Automate Triggers (flow-TriggerCondition)
- **Severity**: Medium
- **Category**: Performance
- **Description**: A common issue for many users who work with Power Automate is that their flow runs whenever a new row or an existing row is modified in the data source. However, we only need the flow to execute when a specific condition is satisfied.
- **Recommendation**: Setting triggers correctly can enable your flow to run only when necessary. Use trigger conditions and OData filter properties to optimize when flows run.

#### 20. Data Retrieval Optimization (flow-DataRetrievalParameters)
- **Severity**: Medium
- **Category**: Performance
- **Description**: In Power Automate, working with only relevant data is crucial for efficiency and accuracy in your automated workflows.
- **Recommendation**: Use Advanced options like Select columns (only allowing flow to get data corresponding to column names specified in the query), Filter rows (to get data based on the Filter condition) and Row count (to only get specified number of rows).

#### 21. Create Update Inside Loop (flow-CreateUpdateInsideLoop)
- **Severity**: Medium
- **Category**: Performance
- **Description**: Often users find using Power Automate to create/updated 1000s of records to the data source when a flow is triggered. Most user would end up using For Each loop which will go through each of these 1000s records, and push them to the data source sequentially causing latency and delays.
- **Recommendation**: 
  - Create/Patch records to the data source in BATCH. Most of the connectors/services expose API services to post request in BATCH
  - Parallelism in For Each loop provides up to 50 records to be processed for the services which do not have capability to accept BATCH requests

#### 22. Asynchronous Flow Pattern (flow-PowerAppResponse)
- **Severity**: Medium
- **Category**: Performance
- **Description**: Generally, if a flow is called from a parent flow or a Power App, it has to send a response back to the calling entity within 120 seconds. If the called flow fails to do that, then the calling entity will receive a timeout and will error out.
- **Recommendation**: Use asynchronous flow pattern for long running flows. In such cases, setting Asynchronous response will allow flow to respond with a 202 to indicate the request has been accepted for processing.

### Coding Standards

#### 23. Consistent Naming for Flow Components (flow-NamingConventions)
- **Severity**: Low
- **Category**: Coding Standards
- **Description**: Maintaining consistent naming conventions for the components within your Power Automate flows is crucial for ensuring clarity, organization, and ease of management.
- **Recommendation**: 
  - **Descriptive and Meaningful Names**: Give meaningful names to your flows before saving
  - **Use CamelCase or Underscores**: Use CamelCase or underscores to separate words in your component names
  - **Prefixes or Tags**: Consider using prefixes or tags to categorize components based on their type or functionality
  - **Consistency Across Flows**: Maintain consistency in naming conventions across all your Power Automate flows
  - **Add Comments**: Adding comments to the actions make it easy to understand the flow implementation logic

#### 24. Multiple Initialize Variable Actions (flow-MultipleInitializeVariableActions)
- **Severity**: Low
- **Category**: Coding Standards
- **Description**: Check if 'Initialize Variable' actions are used more than once. Combine them into one action for efficiency or use parallel initialization for faster execution.
- **Recommendation**: Combine multiple 'InitializeVariable' actions into a single 'InitializeVariable' action of type object or use parallel initialization for faster execution.

### Maintainability

#### 25. Create Scopes (flow-ScopeUsage)
- **Severity**: Low
- **Category**: Maintainability
- **Description**: As workflows get more complicated, it's essential to manage them well to handle problems, test them, and keep them working smoothly. A Scope is like a container that holds a bunch of actions together.
- **Recommendation**: 
  - **Grouping Actions**: You can add multiple actions inside a scope
  - **Organizing Your Flow**: By using scopes, you can create a hierarchical structure within your flow
  - **Error Handling**: Scopes are also helpful for error handling
  - **Visibility and Collapsibility**: Scopes can be collapsed to hide their contents

#### 26. Create Reusable Code (Child flows) (flow-ActionCount)
- **Severity**: Medium
- **Category**: Maintainability
- **Description**: Power Automate cloud flows can help automate complex solutions, however, they can grow excessively big and complex really quick making it difficult to navigate and maintain the flow.
- **Recommendation**: 
  - **Modularity**: Child flows promote modularity, allowing you to build reusable components
  - **Scalability**: By breaking down your automation into smaller, more manageable pieces, you can scale your processes more effectively
  - **Granular Control**: Child flows offer granular control over your automation logic
  - **Collaboration**: In a team environment, child flows promote collaboration

#### 27. Action Note (flow-ActionNote)
- **Severity**: Low
- **Category**: Maintainability
- **Description**: Ensure every action has a non-empty description. Adding meaningful notes/descriptions enhances documentation and maintainability.
- **Recommendation**: Add meaningful notes/descriptions to all actions for better documentation and maintainability.

### Functionality

#### 28. Connection References (flow-ConnectionReferences)
- **Severity**: Low
- **Category**: Functionality
- **Description**: Connection References allow flows to use environment-specific connections without hardcoding credentials, making deployments across environments (Dev, Test, Prod) more manageable. Missing connection references can lead to manual reconfiguration and potential security risks.
- **Recommendation**: Ensure all connection references are correctly configured and utilized.

#### 29. Unused Variables (flow-UnusedVariables)
- **Severity**: Low
- **Category**: Functionality
- **Description**: Check for unused variables in the flow.
- **Recommendation**: Remove any variables that are declared but not used.

#### 30. Trigger Types (flow-TriggerTypes)
- **Severity**: Low
- **Category**: Functionality
- **Description**: Validate the types of triggers used in the flow.
- **Recommendation**: Ensure that the triggers are appropriate for the intended use of the flow.

## Copilot Studio Bot Patterns

### Functionality

#### 31. Duplicate Queries (bot-DuplicateQueries)
- **Severity**: Low
- **Category**: Functionality
- **Description**: This pattern checks for overlapping trigger phrases between topics. If multiple topics have similar or identical phrases, it flags them to avoid conflicts in bot behavior.
- **Recommendation**: Ensure no overlapping trigger queries across topics.

#### 32. Single Word Phrases (bot-SingleWordPhrases)
- **Severity**: Low
- **Category**: Functionality
- **Description**: Single-word trigger phrases should be avoided. Having single-word trigger phrases increase the weight for specific words in topic triggering and can introduce confusion between similar topics.
- **Recommendation**: Ensure all trigger phrases have more than one word. Recommended trigger phrase length is 2-9 words.

#### 33. Trigger Phase Count (bot-TriggerPhraseCount)
- **Severity**: Medium
- **Category**: Functionality
- **Description**: Topics should contain at least 5-10 trigger phrases to ensure that the agent can effectively respond to a wide variety of user inputs. More trigger phrases improve the chances of the agent activating the correct topic.
- **Recommendation**: Ensure each topic has at least 5-10 trigger phrases.

#### 34. Long Phrases (bot-LongPhrases)
- **Severity**: Low
- **Category**: Functionality
- **Description**: Avoid long trigger phrases. Trigger phrases exceeding 10 words in length can have negative impact on the topic recognition and the agent performance.

#### 35. Unused Topics (bot-UnusedTopics)
- **Severity**: Medium
- **Category**: Functionality
- **Description**: Topics that do not have trigger phrases and are not redirected to from other topics, are likely unused and could be removed.
- **Recommendation**: Unused topics clutter the user interface for the maker, and could be a source of confusion. Large amounts of unused topics could have impact on agent performance. After reviewing them and confirming that they are in fact not in use, they should be removed.

#### 36. Synonyms Quality (bot-SynonymsQuality)
- **Severity**: Medium
- **Category**: Functionality
- **Description**: Synonyms should be keywords only, and entering longer phrases as synonyms should be avoided.
- **Recommendation**: Make sure to only enter keywords as synonyms. In most cases they should be single word in length.

#### 37. Duplicate Regex Entities (bot-DuplicateRegexEntities)
- **Severity**: Medium
- **Category**: Functionality
- **Description**: Different entities should not have the same exact regular expression pattern.
- **Recommendation**: Regular expressions for entities should be unique within single agent. Having multiple entities with same pattern can be a source of confusion or conflict.

### Maintainability

#### 38. Condition Count (bot-ConditionCount)
- **Severity**: Medium
- **Category**: Maintainability
- **Description**: Avoid large amount of conditions per topic. Large amount of conditions within single topic can have negative impact on the agent performance.
- **Recommendation**: Consider breaking the topic into subtopics and redirect the conversation to those subtopics to improve maintainability and ensure good performance.

### Performance

#### 39. Flow Count (bot-FlowCount)
- **Severity**: Medium
- **Category**: Performance
- **Description**: Avoid large amount of flows within an agent. Large amount of flows can increase latencies and have negative impact on the end user performance.
- **Recommendation**: Recommendation is to avoid slow logic in a flow and limit the number of flows per agent to 10 or less if possible, to ensure good end user performance.

### Security

#### 40. SharePoint Authentication Mode (bot-SharePointAuthMode)
- **Severity**: High
- **Category**: Security
- **Description**: SharePoint sites cannot be used as knowledge source unless the agent requires/allows authentication.
- **Recommendation**: Ensure that authentication is enabled for agents using SharePoint sites as knowledge source.

### Usage

#### 41. End Of Conversation Usage (bot-EndOfConversationReference)
- **Severity**: High
- **Category**: Usage
- **Description**: Checks if there is at least one End Of Conversation topic (with CancelOtherTopics and a Resolved outcome), and at least one topic referencing it.
- **Recommendation**: Ensure you have an EndOfConversation topic with 'startBehavior: CancelOtherTopics' and conversationOutcome: ResolvedImplied Or conversationOutcome: ResolvedConfirmed and it is referenced by at least one topic to properly end the conversation. Without this, no analytics is collected(no CSAT).

## Pattern Categories

- **Accessibility**: Ensures applications are usable by people with disabilities
- **Coding Standards**: Maintains consistent and readable code practices
- **Functionality**: Ensures proper application behavior and feature implementation
- **Maintainability**: Promotes code that is easy to maintain and modify
- **Performance**: Optimizes application speed and resource usage
- **Security**: Protects against security vulnerabilities
- **UX**: Improves user experience and interface design
- **Usage**: Tracks and optimizes application usage patterns

## Severity Levels

- **High**: Critical issues that significantly impact functionality, security, or performance
- **Medium**: Important issues that should be addressed but don't prevent basic functionality
- **Low**: Minor issues or improvements that enhance code quality or user experience
