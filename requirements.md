# Requirements Document: Bharat MatrixAI

## Introduction

Bharat MatrixAI is a voice-first autonomous agent system designed to eradicate "Interface Poverty" in rural India. The system enables low digital literacy users to interact with English-heavy government portals using vernacular voice commands. The agent autonomously navigates complex web interfaces, extracts information, and provides voice feedback in the user's native language.

The system addresses the critical gap where 95% of government portals are English-only while 90% of rural India speaks vernacular languages. By combining voice AI, computer vision, and autonomous web navigation, Bharat MatrixAI democratizes access to essential government services.

## Glossary

- **System**: The Bharat MatrixAI voice-first autonomous agent platform
- **User**: Rural Indian citizen with low digital literacy speaking vernacular languages
- **Portal**: Government or public service website that the System navigates
- **Intent**: User's goal extracted from vernacular voice input
- **Agent**: Autonomous component that navigates web portals using browser automation
- **Voice_Input_Module**: Component that captures and transcribes vernacular speech
- **Cognitive_Router**: Component that identifies target portals and extracts parameters
- **DOM_Navigator**: Component that performs autonomous web navigation
- **Voice_Output_Module**: Component that synthesizes vernacular speech responses
- **PII**: Personally Identifiable Information requiring privacy protection
- **Vision_Analyzer**: Component that analyzes screenshots to determine navigation actions
- **Task_Session**: Single user interaction from voice input to final response

## Requirements

### Requirement 1: Vernacular Voice Input Capture

**User Story:** As a rural user speaking only vernacular languages, I want to describe my task in my native language, so that I can access government services without knowing English or how to use computers.

#### Acceptance Criteria

1. WHEN a User speaks in a vernacular language (Hindi, Marathi, Tamil, Telugu, Bengali, Gujarati, Kannada, Malayalam, Punjabi, Odia), THE Voice_Input_Module SHALL transcribe the speech to text with >90% accuracy
2. WHEN audio quality is poor or speech is unclear, THE Voice_Input_Module SHALL request the User to repeat their input
3. WHEN the User provides voice input, THE System SHALL process it within 3 seconds of speech completion
4. THE Voice_Input_Module SHALL support continuous listening mode for multi-turn conversations
5. WHEN background noise exceeds acceptable thresholds, THE Voice_Input_Module SHALL apply noise reduction before transcription

### Requirement 2: Intent Understanding and Cognitive Routing

**User Story:** As a user, I want the system to understand what I'm trying to accomplish, so that it can automatically navigate to the correct government portal and extract the necessary information.

#### Acceptance Criteria

1. WHEN the Cognitive_Router receives transcribed vernacular text, THE System SHALL identify the target Portal with >95% accuracy
2. WHEN the intent requires specific parameters (dates, names, ID numbers), THE Cognitive_Router SHALL extract all required parameters from the voice input
3. IF required parameters are missing, THEN THE System SHALL ask clarifying questions in the User's vernacular language
4. THE Cognitive_Router SHALL map intents to Portal navigation strategies without requiring code changes for new Portals
5. WHEN multiple Portals could satisfy the intent, THE Cognitive_Router SHALL select the most appropriate Portal based on User context and intent specificity

### Requirement 3: Omnichannel Ingestion

**User Story:** As a rural user comfortable with WhatsApp, I want to send voice notes directly through WhatsApp Business API, so that I can access government services using the messaging platform I already use daily.

#### Acceptance Criteria

1. WHEN a User sends a voice note via WhatsApp Business API, THE System SHALL receive the audio payload through AWS API Gateway
2. THE System SHALL support voice note ingestion from both the React Native mobile app AND WhatsApp Business API without requiring separate backend logic
3. WHEN a voice note is received via WhatsApp, THE System SHALL extract the user's phone number for session identification and response routing
4. THE System SHALL route WhatsApp-originated requests through the same Step Functions workflow as mobile app requests
5. WHEN the System completes processing a WhatsApp-originated request, THE System SHALL send the voice response back to the user via WhatsApp Business API
6. THE System SHALL maintain conversation context across multiple WhatsApp messages within a Task_Session
7. WHEN a User switches between mobile app and WhatsApp mid-conversation, THE System SHALL maintain session continuity using phone number as the identifier

### Requirement 4: Autonomous Web Navigation

**User Story:** As a user, I want the system to automatically navigate government websites on my behalf, so that I don't need to understand complex web interfaces or English forms.

#### Acceptance Criteria

1. WHEN the DOM_Navigator receives a Portal target and parameters, THE System SHALL launch a browser session and navigate to the Portal URL
2. WHEN the DOM_Navigator encounters a form field, THE System SHALL identify the field purpose using Vision_Analyzer and input the appropriate parameter value
3. WHEN the DOM_Navigator encounters interactive elements (buttons, dropdowns, links), THE System SHALL determine the correct element to interact with using coordinate-based clicking
4. WHEN a Portal page takes longer than 15 seconds to load, THE System SHALL wait for page completion before proceeding
5. WHEN the DOM_Navigator encounters CAPTCHAs or authentication challenges, THE System SHALL notify the User and request manual intervention
6. THE DOM_Navigator SHALL capture screenshots at each navigation step for vision-based analysis
7. WHEN Portal structure changes (new layouts, moved elements), THE System SHALL adapt navigation using Vision_Analyzer without requiring code updates

### Requirement 5: Vision-Based Navigation Intelligence

**User Story:** As a system operator, I want the navigation to be resilient to website changes, so that the system continues working even when government portals update their interfaces.

#### Acceptance Criteria

1. WHEN the Vision_Analyzer receives a screenshot, THE System SHALL identify all interactive elements and their purposes
2. WHEN determining where to click, THE Vision_Analyzer SHALL return precise X/Y coordinates for the target element
3. WHEN the Vision_Analyzer cannot identify the required element with >80% confidence, THE System SHALL request human guidance
4. THE Vision_Analyzer SHALL recognize common UI patterns (forms, tables, buttons, navigation menus) across different Portal designs
5. WHEN extracting information from Portal pages, THE Vision_Analyzer SHALL identify and extract relevant text content with >95% accuracy

### Requirement 6: Information Extraction and Result Synthesis

**User Story:** As a user, I want to receive the information I requested in a clear and understandable format, so that I can make informed decisions about my government services.

#### Acceptance Criteria

1. WHEN the DOM_Navigator completes Portal navigation, THE System SHALL extract all relevant information matching the User's intent
2. WHEN extracting tabular data, THE System SHALL preserve the structure and relationships between data elements
3. THE System SHALL format extracted information into natural language suitable for voice synthesis
4. WHEN extracted information contains technical terms or English text, THE System SHALL translate to the User's vernacular language
5. WHEN information extraction fails or returns incomplete data, THE System SHALL inform the User of the specific missing information

### Requirement 7: Vernacular Voice Output

**User Story:** As a user, I want to hear the results in my native language through natural-sounding speech, so that I can understand the information without reading or knowing English.

#### Acceptance Criteria

1. WHEN the System has results to communicate, THE Voice_Output_Module SHALL synthesize speech in the User's vernacular language
2. THE Voice_Output_Module SHALL produce natural-sounding speech with appropriate prosody and pacing
3. WHEN communicating complex information, THE Voice_Output_Module SHALL break content into digestible segments with appropriate pauses
4. THE Voice_Output_Module SHALL support all vernacular languages supported by the Voice_Input_Module
5. WHEN the User requests repetition, THE Voice_Output_Module SHALL replay the previous response

### Requirement 8: Asynchronous Task Handling

**User Story:** As a user, I want to be notified when my task is complete even if it takes a long time, so that I don't have to wait on the phone for slow government websites.

#### Acceptance Criteria

1. WHEN a Task_Session is expected to exceed 30 seconds, THE System SHALL offer asynchronous processing with callback notification
2. WHEN processing asynchronously, THE System SHALL provide the User with a task reference identifier
3. WHEN an asynchronous task completes, THE System SHALL notify the User via their preferred channel (voice call, SMS)
4. THE System SHALL maintain Task_Session state across disconnections and reconnections
5. WHEN the User checks task status, THE System SHALL provide progress updates in vernacular language

### Requirement 9: Privacy and PII Protection

**User Story:** As a user sharing personal information, I want my data to be protected, so that my privacy is maintained and sensitive information doesn't leak.

#### Acceptance Criteria

1. WHEN the System logs any data, THE System SHALL redact all PII (names, addresses, phone numbers, ID numbers, dates of birth) before writing to logs
2. THE System SHALL encrypt all User data in transit using TLS 1.3 or higher
3. THE System SHALL encrypt all User data at rest using AES-256 encryption
4. WHEN a Task_Session completes, THE System SHALL delete all temporary data including screenshots and browser session data within 24 hours
5. THE System SHALL not store voice recordings beyond the duration required for transcription
6. WHEN PII redaction fails or is uncertain, THE System SHALL err on the side of caution and redact the questionable content

### Requirement 10: Horizontal Scalability

**User Story:** As a system administrator, I want to add support for new government portals without changing core code, so that the system can scale to cover all government services efficiently.

#### Acceptance Criteria

1. WHEN adding a new Portal, THE System SHALL require only configuration data (Portal URL, intent keywords, expected workflow) without code changes
2. THE System SHALL support concurrent navigation of multiple Portals for different Users without interference
3. WHEN User load increases, THE System SHALL automatically scale navigation capacity by spawning additional browser instances
4. THE System SHALL maintain per-Portal configuration in a centralized registry accessible to all components
5. WHEN Portal configurations are updated, THE System SHALL apply changes without requiring system restart

### Requirement 11: Cost Efficiency

**User Story:** As a service provider, I want to keep per-transaction costs low, so that the service remains financially sustainable and accessible to rural users.

#### Acceptance Criteria

1. WHEN processing a Task_Session, THE System SHALL complete the transaction for less than $0.03 in infrastructure costs
2. THE System SHALL optimize AI model usage by caching common intent patterns and Portal strategies
3. THE System SHALL terminate browser sessions immediately after task completion to minimize compute costs
4. WHEN multiple Users request the same Portal information within a time window, THE System SHALL cache and reuse results where appropriate
5. THE System SHALL monitor per-transaction costs and alert administrators when costs exceed $0.03

### Requirement 12: Reliability and Error Handling

**User Story:** As a user with limited technical knowledge, I want clear guidance when something goes wrong, so that I understand what happened and what to do next.

#### Acceptance Criteria

1. WHEN Portal navigation fails, THE System SHALL provide the User with a clear explanation in vernacular language
2. WHEN the System encounters an error, THE System SHALL log sufficient diagnostic information for troubleshooting while respecting PII redaction requirements
3. IF a Portal is temporarily unavailable, THEN THE System SHALL inform the User and offer to retry or schedule the task for later
4. THE System SHALL achieve >85% task completion rate across all supported Portals
5. WHEN the System cannot complete a task autonomously, THE System SHALL provide the User with alternative options (human assistance, simplified guidance)

### Requirement 13: Multi-Turn Conversation Support

**User Story:** As a user, I want to have a natural conversation with the system, so that I can clarify my needs and provide additional information as requested.

#### Acceptance Criteria

1. WHEN the System needs additional information, THE System SHALL ask follow-up questions in the User's vernacular language
2. THE System SHALL maintain conversation context across multiple voice exchanges within a Task_Session
3. WHEN the User provides additional information, THE System SHALL integrate it with previously captured intent and parameters
4. THE System SHALL support conversation repair when the User corrects or changes previously stated information
5. WHEN the User says "go back" or equivalent vernacular phrases, THE System SHALL return to the previous conversation state

## Success Metrics

- Task completion rate: >85%
- Voice transcription accuracy: >90%
- Intent recognition accuracy: >95%
- Information extraction accuracy: >95%
- Cost per transaction: <$0.03
- Average task completion time: <60 seconds for synchronous tasks
- User satisfaction: >4.0/5.0 in vernacular language surveys
