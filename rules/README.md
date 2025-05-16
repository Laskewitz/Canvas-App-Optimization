# Rules for the Power CAT Tools - Code Review Tool

This document provides a categorized list of patterns and recommendations for improving the quality, performance, and maintainability of your Power Platform solutions. The rules are grouped into sections for Power Automate, Power Apps, and Copilot Studio to help you quickly find relevant guidance.

## Copilot Studio

### bot-TriggerPhraseCount
- **Category**: Functionality
- **Severity**: Medium
- **Description**: Topics should contain at least 5-10 trigger phrases to ensure that the agent can effectively respond to a wide variety of user inputs. More trigger phrases improve the chances of the agent activating the correct topic.
- **Recommendation**: Ensure each topic has at least 5-10 trigger phrases.
- **Documentation**: [Link](https://learn.microsoft.com/microsoft-copilot-studio/guidance/trigger-phrases-best-practices#2)

### bot-LongPhrases
- **Category**: Functionality
- **Severity**: Low
- **Description**: Avoid long trigger phrases. Trigger phrases exceeding 10 words in length can have negative impact on the topic recognition and the agent performance.
- **Documentation**: [Link](https://learn.microsoft.com/microsoft-copilot-studio/guidance/trigger-phrases-best-practices#4)

### bot-ConditionCount
- **Category**: Functionality
- **Severity**: Medium
- **Description**: Avoid large amount of conditions per topic. Large amount of conditions within single topic can have negative impact on the agent performance.
- **Recommendation**: Consider breaking the topic into subtopics and redirect the conversation to those subtopics to improve maintainability and ensure good performance.
- **Documentation**: [Link](https://learn.microsoft.com/microsoft-copilot-studio/guidance)

### bot-FlowCount
- **Category**: Functionality
- **Severity**: Medium
- **Description**: Avoid large amount of flows within an agent. Large amount of flows can increase latencies and have negative impact on the end user performance.
- **Recommendation**: Recommendation is to avoid slow logic in a flow and limit the number of flows per agent to 10 or less if possible, to ensure good end user performance.

### bot-SharePointAuthMode
- **Category**: Security
- **Severity**: High
- **Description**: SharePoint sites cannot be used as knowledge source unless the agent requires/allows authentication.
- **Recommendation**: Ensure that authentication is enabled for agents using SharePoint sites as knowledge source.
- **Documentation**: [Link](https://learn.microsoft.com/microsoft-copilot-studio/guidance)

### bot-UnusedTopics
- **Category**: Functionality
- **Severity**: Medium
- **Description**: Topics that do not have trigger phrases and are not redirected to from other topics, are likely unused and could be removed.
- **Recommendation**: Unused topics clutter the user interface for the maker, and could be a source of confusion. Large amounts of unused topics could have impact on agent performance. After reviewing them and confirming that they are not in use, remove them.
- **Documentation**: [Link](https://learn.microsoft.com/microsoft-copilot-studio/guidance)

### bot-SynonymsQuality
- **Category**: Functionality
- **Severity**: Medium
- **Description**: Synonyms should be keywords only, and entering longer phrases as synonyms should be avoided.
- **Recommendation**: Make sure to only enter keywords as synonyms. In most cases they should be single word in length.
- **Documentation**: [Link](https://learn.microsoft.com/microsoft-copilot-studio/guidance)

### bot-DuplicateRegexEntities
- **Category**: Functionality
- **Severity**: Medium
- **Description**: Different entities should not have the same exact regular expression pattern.
- **Recommendation**: Regular expressions for entities should be unique within single agent. Having multiple entities with same pattern can be a source of confusion or conflict.
- **Documentation**: [Link](https://learn.microsoft.com/microsoft-copilot-studio/guidance)

### bot-EndOfConversationReference
- **Category**: Usage
- **Severity**: High
- **Description**: Checks if there is at least one End Of Conversation topic (with CancelOtherTopics and a Resolved outcome), and at least one topic referencing it.
- **Recommendation**: Ensure you have an EndOfConversation topic with 'startBehavior: CancelOtherTopics' and conversationOutcome: ResolvedImplied Or conversationOutcome: ResolvedConfirmed and it is referenced by at least one topic to properly end the conversation.
- **Documentation**: [Link](https://learn.microsoft.com/microsoft-copilot-studio/guidance)

## Power Apps

### app-FormulaOptimization-NestedFilter
- **Category**: Performance
- **Severity**: Medium
- **Description**: Consider simplifying nested formulas with Search() and Filter() operations. Avoiding the nesting of these two together leads to a more compact/concise formula and performance improvement.
- **Recommendation**: Simplify nested filters to improve performance. Combine multiple conditions into a single `Filter()` or `Search()` call to optimize data retrieval. Where possible, apply filtering at the data source level (e.g., using delegation-friendly queries) to improve performance and reduce processing load in Power Apps.

### app-UxLayout
- **Category**: UX
- **Severity**: Low
- **Description**: This pattern checks for use of layout containers in your app to determine if your app is responsive.
- **Recommendation**: Using layout containers can significantly enhance the design and functionality of your apps. Responsive design, organized structure, improved accessibility, and enhanced performance are key benefits.
- **Documentation**: [Link](https://learn.microsoft.com/powerapps/maker/canvas-apps/create-responsive-layout)

### app-InefficientDelayLoading
- **Category**: Performance
- **Severity**: Medium
- **Description**: Cross page references force the load of additional screens if one screen has a dependency on a control in a different screen.
- **Recommendation**: Keep cross screen dependencies between screens low. Use variable & collection rather than values from controls properties.
- **Documentation**: [Link](https://learn.microsoft.com/power-apps/maker/canvas-apps/fast-app-page-load)

### app-UnusedMediaResources
- **Category**: Performance
- **Severity**: Low
- **Description**: The app contains media (.jpg, .jpeg, .gif, .png, .bmp, .tif, .tiff, .svg, .wav, .mp3, .mp4) that have been uploaded to the app but not used in any screen.
- **Recommendation**: You can remove all unused media from the app to clean up or reduce the size of the app using the "Remove unused media" option.
- **Documentation**: [Link](https://learn.microsoft.com/power-apps/maker/canvas-apps/add-images-pictures-audio-video)

## Power Automate

### flow-TriggerTypes
- **Category**: Functionality
- **Severity**: Low
- **Description**: Validate the types of triggers used in the flow.
- **Recommendation**: Ensure that the triggers are appropriate for the intended use of the flow.
- **Documentation**: [Link](https://learn.microsoft.com/power-automate/trigger-types)

### flow-MultipleInitializeVariableActions
- **Category**: Coding Standards
- **Severity**: Low
- **Description**: Check if 'Initialize Variable' actions are used more than once. Combine them into one action for efficiency or use parallel initialization for faster execution.
- **Recommendation**: Combine multiple 'InitializeVariable' actions into a single 'InitializeVariable' action of type object or use parallel initialization for faster execution.
- **Documentation**: [Link](https://learn.microsoft.com/power-automate/guidance/coding-guidelines/use-data-operations#json-variables-vs-individual-variables)

### flow-ActionNote
- **Category**: Maintainability
- **Severity**: Low
- **Description**: Ensure every action has a non-empty description. Adding meaningful notes/descriptions enhances documentation and maintainability.
- **Recommendation**: Add meaningful notes/descriptions to all actions for better documentation and maintainability.
- **Documentation**: [Link](https://learn.microsoft.com/power-automate/guidance/coding-guidelines/use-peekcode-addnotes#add-notes)
