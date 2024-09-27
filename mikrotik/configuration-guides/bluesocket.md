# **Bluesocket**

---

This page explains the configuration of Bluesocket wireless access points for external Captive  Portal and RADIUS server authentication.

### IronWifi Console Configuration

1. Log into the IronWifi console or [register for free](https://console.ironwifi.com/register)
2. Create a **new network**
3. After that, create a **new captive portal**, with vendor **Bluesocket**

### Access Point Configuration

Please log in to your Bluesocket WLAN controller

At the top click on **Configuration** and then on the left, under **External Authentication** click on **Accounting**

Click on **Create Accounting Server** and enter the following:

- **Name**<span>: guest1</span>
- **Enabled**<span>: Ticked</span>
- **IP Address**<span>: {{primary_ip}}</span>
- **Port**<span>: 1813</span>
- **Shared Secret**<span>: {{shared_secret}}</span>
- **Shared Secret Confirmation**<span>: as above</span>
- **Timeout**<span>: 5</span>
- **Retries**<span>: 5</span>
- **Interim Updates Enabled**<span>: Ticked</span>
- **Interim Update Interval**<span>: 300</span>

Click **Create Accounting Server**

Click on **Create Accounting Server** again and enter the following:

- **Name**<span>: guest2</span>
- **Enabled**<span>: Ticked</span>
- **IP Address**<span>: {{backup_ip}}</span>
- **Port**<span>: 1813</span>
- **Shared Secret**<span>: {{shared_secret}}</span>
- **Shared Secret Confirmation**<span>: as above</span>
- **Timeout**<span>: 5</span>
- **Retries**<span>: 5</span>
- **Interim Updates Enabled**<span>: Ticked</span>
- **Interim Update Interval**<span>: 300</span>

Click **Create Accounting Server**

Next, on the left, under **External Authentication** click on **Servers**. Click on **Create Authentication Server** and enter the following:

- **Type**<span>: RadiusWebAuthServer</span>
- **Name**<span>: guest1</span>
- <span>**Accounting Server**</span><span>: guest1</span>
- **IP Address**<span>: {{primary_ip}}</span>
- **Port**<span>: 1812</span>
- **Shared Secret**<span>: {{shared_secret}}</span>
- **Shared Secret Confirmation**<span>: as above</span>
- **Timeout Weight**<span>: 1</span>
- **Precedence**<span>: Highest</span>
- **Role**: Guest

Click on **Create Authentication Server**.

<span style="font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 13px;">Click on </span>**Create Authentication Server**<span style="font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 13px;"> again and enter the following:</span>

- **Type**<span>: RadiusWebAuthServer</span>
- **Name**<span>: guest2</span>
- <span>**Accounting Server**</span><span>: guest2</span>
- **IP Address**<span>: {{backup_ip}}</span>
- **Port**<span>: 1812</span>
- **Shared Secret**<span>: {{shared_secret}}</span>
- **Shared Secret Confirmation**<span>: as above</span>
- **Timeout Weight**<span>: 1</span>
- **Precedence**<span>: Lowest</span>
- **Role**: Guest

<span>Click on </span>**Create Authentication Server**<span>.</span>

Next, on the left under **Captive Portal**, click on **Forms**. Click **Create Login Form** and enter the following:

- **Name**<span>: guest</span>
- **Allow User Logins**<span>: Ticked</span>
- **Allow Guest Logins**<span>: Unticked</span>
- **Redirect Clients to an External URL**<span>: Ticked</span>
- **Base URL of External Server**<span>: {{splash_page_url}}</span>
- **Clients Access Point MAC Address**<span>: blue\_ap</span>
- **Client's Access Point Name**<span>: blue\_ap\_name</span>
- **vWLAN IP Address**<span>: blue\_controller</span>
- **Client's Original URL**<span>: blue\_destination</span>
- **Client's MAC Address**<span>: blue\_mac</span>
- **Client's IP Address**<span>: blue\_source</span>
- **Client's Access Point SSID**<span>: blue\_ssid</span>
- **Client's VLAN ID**<span>: blue\_vlan</span>
- **Double Encoding of URI Parameters**<span>: Unticked</span>
- **Include RADIUS Option Vendor option**<span>: Unticked</span>

Click on **Create Login Form**.

Next, on the left, under **Role Based Access Control** click on **Destinations**. Click on **Create Destination Hostname** and enter:

- **Name**<span>: guestportal</span>
- **Address**<span>: \*insert access\_domain here\*</span>

Click on **Create Destination**. Now, for each of the below entries, create another destination hostname until you have added each one:

107.178.250.42
  
If you need to load resources from external servers (SAML, social login), you will need to add other entries as well, instructions to configure the walled garden list in this case are available [here](https://ironwifi.com/walled-garden-list-guide).


Next, on the left, click on **Destination Groups**. Click on **Create Destination Group**.

- **Name**<span>: guest</span>
- **Destinations**<span>: Click the + sign beside each domain on the right hand list to add all of these to the left list. Be sure not to add the "Any" rule.</span>

Click **Create Destination Group**

Next, on the left, click on **Roles**. Click on the **Un-registered** role. At the bottom, click on **Append Firewall Rule** and choose:

- **Policy**<span>: Allow</span>
- **Service**<span>: Any</span>
- **Direction**<span>: Both Ways</span>
- **Destination**<span>: under "Destination Groups" choose </span>**guest**

Click **Update Role**.

<span>Next, on the left, click on </span>**Roles**. Click on the **Guest** role. Under the **Post Login Redirection** section, enter:

**URL Redirect**: {{success_page_url}}

Click **Update Role** to save.

Next, on the left, under **Wireless** click on **SSIDs**. Click on **Create SSID** and enter the following:

**Name**: Guest WiFi (or whatever you wish)

**Broadcast SSID**: Ticked

**Authentication**: Open System

**Cipher**: Disabled

**Login Form**: guest

**Role**: Un-registered

**Standby SSID**: Unticked

Click on **Create SSID**.

Finally, you need to apply this new configuration to your AP's in the usual way. For example, go to the **Status** tab at the top and choose **Access Points**. Highlight the ones you are using and click the **Apply** button.

 ! You must also install a valid SSL certificate on your controller/AP, in order to avoid authentication issues !
