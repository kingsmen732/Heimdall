![title-pic](https://github.com/kingsmen732/heimdall/blob/main/demo.png)

# Captive Portal Connection Manager

This project automates the process of detecting and connecting to captive portals, commonly used in public Wi-Fi networks, where users need to log in to access the internet. It handles everything from finding the portal, logging in using stored credentials, maintaining connectivity, and sending acknowledgment requests to ensure continued access.

## Features
- Detects when the internet (WAN) is down and checks for the presence of a captive portal.
- Automatically logs into the captive portal with stored credentials.
- Supports both `DESKTOP` and `MOBILE` modes, with automatic User-Agent switching.
- Periodically sends acknowledgment requests to maintain connectivity in `DESKTOP` mode.
- Provides a user interface with options to log out, change client type, or resume the script.
- Comprehensive logging of events such as connection status, login attempts, and server responses.

## How It Works
1. **Credential Management**: The script reads credentials (username and password) from a `credentials.json` file.
2. **Connectivity Tests**: It first checks whether the WAN (internet) is up by running connectivity tests to Google's and Microsoft's network test sites.
3. **Captive Portal Detection**: If the internet is down, the script attempts to find a captive portal by connecting to Microsoft's `connecttest.txt`. If redirected, it assumes a portal was found.
4. **Auto-Login**: If a captive portal is detected, it sends a POST request to log in using the stored credentials.
5. **Acknowledgment**: In `DESKTOP` mode, the script sends periodic GET requests to acknowledge the connection and maintain the session.
6. **User Interaction**: The script provides a console-based menu where the user can choose to log out, change client type, or exit the script.

## Enums and Classes

### Enums
- **`Client_type`**: Specifies whether the client is using a `MOBILE` or `DESKTOP` device.
- **`Request_type`**: Defines whether the request is a `POST` or `GET`.
- **`Portal_action`**: Enumerates the different actions within the portal (e.g., `LOGIN`, `LOGOUT`, `ACK`).
- **`Server_response`**: Specifies server responses such as `LOGIN_SUCCESSFUL`, `LOGOUT_SUCCESSFUL`, `MAX_DEVICES_REACHED`, and `INVALID_CREDENTIALS`.

### `Global` Class
A central repository for global variables used throughout the script, including user credentials, portal URL, timestamps, WAN state, and user agent.

### `Utility` Class
A collection of helper methods that manage:
- Credential loading (`get_creds_from_json`)
- Connectivity tests (`gstatic_connect_test`, `msft_connect_test`)
- Captive portal detection (`find_captive_portal`)
- HTTP header and payload generation (`header_generator`, `payload_generator`)
- WAN state tracking (`is_wan_up`)
- Console UI rendering (`print_logo`, `print_states`)
- Client type switching (`change_client_type`)

## HTTP Request Functions
- **`get_req(uni_host)`**: Sends a GET request to acknowledge the connection.
- **`post_req(uni_host, portal_action)`**: Sends a POST request for login or logout.
- **`parse_server_response(response)`**: Parses the server's response and logs the result (e.g., login success, invalid credentials).

## Flow of Execution
1. **Program Start**: The script loads credentials, determines the client type, and enters the main loop.
2. **WAN Check**: It continuously checks if the WAN is up using Google’s or Microsoft’s connectivity test.
3. **Captive Portal Detection**: If the WAN is down, the script looks for a captive portal and tries to auto-login.
4. **WAN Up**: If the WAN is up and the client type is `DESKTOP`, it sends periodic acknowledgment requests.
5. **User Interaction**: A menu allows the user to log out, change client type, or exit the script.

## Getting Started

### Prerequisites
- Python 3.x
- `requests` library

Install the `requests` library using pip:
```bash
pip install requests
```

### Setting Up
1. Clone this repository.
2. Create a `credentials.json` file in the root directory with the following format:
    ```json
    {
      "username": "your-username",
      "password": "your-password"
    }
    ```
3. Run the script:
    ```bash
    python main.py
    ```

### Running the Script
- The script will automatically detect if a captive portal is present and log in using the credentials provided in the `credentials.json` file.
- If the WAN is up, the script will maintain connectivity by sending acknowledgment requests (in `DESKTOP` mode).
- Press `Ctrl + C` to stop the script and bring up the menu.

### Example `credentials.json`
```json
{
  "username": "test_user",
  "password": "password123"
}
```

## Logging
Logs are saved in `app.log` and include details such as:
- WAN state changes
- Login/logout success or failure
- Errors and exceptions

## License
This project is licensed under the MIT License.
