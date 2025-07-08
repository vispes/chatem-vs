# ChatEm Chrome Extension

## Project Description

ChatEm is a Chrome extension designed to enhance user experience by integrating chat functionalities directly within the Chrome browser environment. This project aims to provide seamless chat capabilities, likely for customer support or team collaboration, by leveraging background processing, content script injection, and a user-friendly interface that interacts with web pages.

## Overview

ChatEm aims to revolutionize how users communicate and interact while browsing the web. Whether for customer support, internal team discussions, or collaborative tasks, this extension provides a persistent and accessible chat interface without the need to switch away from the current tab. It integrates with web pages to offer contextual information and embedded chat features, making communication more efficient and context-aware.

## Installation

To install the ChatEm Chrome Extension:

1.  **Clone the repository:**
    ```bash
    git clone <repository_url>
    cd chatem-vs
    ```
2.  **Open Chrome Extensions:**
    *   Open Google Chrome.
    *   Navigate to `chrome://extensions/` by typing it in the address bar.
3.  **Enable Developer Mode:**
    *   In the top-right corner of the Extensions page, toggle the "Developer mode" switch to the ON position.
4.  **Load the unpacked extension:**
    *   Click the "Load unpacked" button in the top-left corner.
    *   Navigate to the `chatem-vs` directory where you cloned the repository and select it.

The ChatEm extension should now appear in your list of installed extensions, and its icon will be visible in the Chrome toolbar.

## Usage

### Interacting with the Chat

1.  **Accessing the Popup:** Click the ChatEm extension icon in your Chrome toolbar. This will open the popup window, which serves as the primary interface for quick actions and status updates.
2.  **In-Page Chat Widget:** On supported websites, a chat widget will be dynamically injected by the content script. This widget allows for direct interaction with chat functionalities embedded within the web page context.
3.  **Configuration:** Navigate to the extension's options page (usually accessible by right-clicking the extension icon and selecting "Options" or via the `chrome://extensions/` page) to customize settings such as appearance, notification preferences, and integration options.

### Key Functionalities

*   **Real-time Chat:** Engage in conversations directly within your browser.
*   **Contextual Integration:** Chat functionalities are seamlessly integrated with the web pages you visit.
*   **User Management:** Authenticate and manage your user profile.
*   **Conversation History:** Access past conversations for reference.
*   **Notifications:** Receive timely alerts for new messages.
*   **Customization:** Tailor the extension's appearance and behavior to your preferences.

## Project Architecture

ChatEm follows a standard Chrome Extension architecture to ensure robust functionality and efficient communication between different parts of the extension:

*   **Background Script (`background.js`):** The core of the extension, handling long-running tasks, event listening (e.g., tab updates, messages), and managing the extension's overall state. It acts as a central communication hub.
*   **Content Scripts (`content.js`):** Injected into web pages to interact with the Document Object Model (DOM), extract information, and dynamically inject UI elements like the chat widget. They communicate with the background script via message passing.
*   **Popup/Options Pages (HTML/CSS/JS):**
    *   **Popup:** Provides a quick, accessible UI for immediate actions and status display when the extension icon is clicked.
    *   **Options Page:** Offers comprehensive settings for users to configure the extension's behavior, appearance, and integrations.
*   **Content UI (HTML/CSS/JS):** Refers to the UI elements (e.g., chat widget) that are directly injected into web pages by the content scripts.

## Project Flow

1.  **Initialization:** Upon browser startup or extension installation, `background.js` initializes and sets up necessary listeners for Chrome API events.
2.  **User Interaction:** Users interact with the popup or options pages. Scripts associated with these UI elements send messages to the `background.js` script.
3.  **Content Script Injection:** Based on `manifest.json` configurations, `content.js` is injected into applicable web pages.
4.  **Content Interaction:** `content.js` interacts with the web page's DOM, potentially gathering data or launching the chat widget.
5.  **Inter-Script Communication:** All parts of the extension (popup, options, content scripts) communicate with the `background.js` script using `chrome.runtime.sendMessage` and `chrome.runtime.onMessage`.
6.  **Data Handling:** `background.js` manages data persistence (e.g., using `chrome.storage`) and interfaces with external APIs or services via `apiservice.js`.
7.  **UI Updates:** The background script can initiate UI updates by sending messages to the popup, options page, or content scripts, based on received data or triggered events.

## Components

| Component Name      | File Type | Status | Research Notes                                                                                           | Pseudo-code Snippet                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Dependencies          |
| :------------------ | :-------- | :----- | :------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :-------------------- |
| `manifest`          | `.json`   | Pass   | The core configuration file for the Chrome Extension. Defines extension name, version, permissions, scripts to run, and UI pages. | `// no signatures detected`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |                       |
| `background`        | `.js`     | Pass   | Handles background tasks, event listening, and inter-script communication. Manages the extension's core logic and state. | `initListeners()\nhandleMessage(message, sender, sendResponse)\nonTabUpdated(tabId, changeInfo, tab)\nonExtensionInstalled()\nsendMessageToContent(tabId, message)\nsendMessageToPopup(message)\ngetUserProfile()\nsaveConversationHistory(conversation)\ngetConversationHistory()\nfetchFromApi(endpoint, options)\nsaveSettings(settings)\nloadSettings()`                                                                                                                                                                             |                       |
| `content`           | `.js`     | Pass   | Injected into web pages to interact with the DOM, extract information, and potentially inject UI elements. | `init()\nonMessage(message, sender, sendResponse)\nsendMessageToBackground(message)\ninjectChatWidget(config)\nremoveChatWidget()\nupdateChatWidget(data)\nextractPageData()\ndomReadyListener()`                                                                                                                                                                                                                                                                                                                                                                                                      |                       |
| `popup`             | `.html`   | Pass   | The HTML structure for the extension's popup UI, which appears when the extension icon is clicked.      | `// no signatures detected`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | `popup.js`            |
| `popup`             | `.js`     | Pass   | JavaScript logic for the popup UI, handling user interactions and communication with the background script. | `init()\nonMessage(message, sender, sendResponse)\nsendMessageToBackground(message)\nrenderChatHistory(history)\nrenderUserProfile(profile)\nupdateNotification(message)\nsendMessageToContentScript(message)\nopenOptionsPage()`                                                                                                                                                                                                                                                                                                                                                                                                   | `popup.html`          |
| `popup`             | `.css`    | Pass   | Styling for the extension's popup UI.                                                                    | `// no signatures detected`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | `popup.html`          |
| `options`           | `.html`   | Pass   | The HTML structure for the extension's options page, allowing users to configure settings.             | `// no signatures detected`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | `options.js`          |
| `options`           | `.js`     | Pass   | JavaScript logic for the options page, handling user input and saving settings.                          | `init()\nonMessage(message, sender, sendResponse)\nsendMessageToBackground(message)\nloadUserSettings()\nsaveUserSettings()\nrenderSettingsForm(settings)\nupdateSetting(key, value)\nopenChatWidget()`                                                                                                                                                                                                                                                                                                                                                                                                                         | `options.html`        |
| `options`           | `.css`    | Pass   | Styling for the extension's options page.                                                                | `// no signatures detected`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | `options.html`        |
| `chatwidget`        | `.js`     | Pass   | JavaScript to dynamically create and manage the chat interface injected into web pages.                  | `init(config)\nrender()\ndisplayMessage(message)\nsendMessage(messageContent)\nonMessageFromBackground(message)\ntoggleVisibility()\nupdateWidgetUI(data)\nattachEventListeners()`                                                                                                                                                                                                                                                                                                                                                                                                                         | `content.js`          |
| `chatwidget`        | `.css`    | Pass   | Styling for the chat widget injected into web pages.                                                     | `// no signatures detected`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | `content.js`          |
| `apiservice`        | `.js`     | Pass   | Utility file to handle interactions with external APIs or backend services for chat.                   | `init(baseUrl)\nlogin(credentials)\nregister(userData)\nsendMessage(messageData)\ngetConversations(userId)\ngetUserInfo(userId)\nfetchData(endpoint, options)`                                                                                                                                                                                                                                                                                                                                                                                                                                                     | `background.js`       |
| `storageservice`    | `.js`     | Pass   | Utility file for managing extension data using `chrome.storage` (local or sync).                         | `saveItem(key, value)\ngetItem(key)\nremoveItem(key)\nclearAll()`                                                                                                                                                                                                                                                                                                                                                                                                                                                           | `background.js`       |

*Note: Some components might appear multiple times due to the way the provided "Components" list was structured (e.g., duplicate entries for `background`, `content`, etc.). The "Status" and "Research Notes" generally reflect the primary function of each component type.*

## Dependencies

This project relies on the following:

*   **Google Chrome Browser:** The extension is built for the Chrome browser.
*   **Chrome Extension APIs:** Utilizes various Chrome APIs for functionalities like message passing, storage, tabs, and more.
*   **JavaScript, HTML, CSS:** Standard web development technologies for building the extension's logic and UI.
*   **Potential External APIs/Services:** The `apiservice.js` component suggests integration with backend services or third-party APIs for chat functionalities, user authentication, etc. (specific dependencies will depend on the chosen backend).

## Contributing

Contributions are welcome! Please refer to the `CONTRIBUTING.md` file (if available) for guidelines on how to contribute to this project.

## License

This project is licensed under the MIT License - see the `LICENSE` file for details.

## Further Information

For a more detailed understanding of the project's scope, goals, and specific features, please refer to the project documentation: [Project Description Document](https://docs.google.com/document/d/1eiJ6KLUiLG3ooNlLi3HhURaHGZy_X0t95gt48aVTVtU/edit?usp=sharing)