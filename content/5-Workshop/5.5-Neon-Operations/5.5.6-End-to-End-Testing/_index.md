---
title : "End-to-End Testing"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.5.6. </b> "
---

After completing this section, the system will be verified to ensure:
- The complete user flow from **Frontend** through **ALB** to **Backend (ECS)** works correctly
- Storage and CDN services (**S3, CloudFront, RDS MySQL**) serve data as expected
- No errors, broken links, or data issues exist in core features


### 5.5.6.1. Test Scenarios

#### Scenario 1. User Registration → Login Flow

| Field | Detail |
|-------|--------|
| **Objective** | Ensure account creation and login succeed end-to-end |
| **Expected result** | User creates an account successfully, logs in, and the system updates status correctly |

**Steps:**

1. Access the web via CloudFront.
2. Go to **Settings**, select **"Sign in with another account"** → click **Create account**.
3. Fill in all required fields in the registration form.
4. Use the newly created account to **Sign in**.

**Edge cases:**
- Case 1: Unable to open the transaction gateway due to a system error.
- Case 2: Incorrect information filled in — form shows validation error.
- Case 3: Login fails due to incorrect password.

![Figure 1. Registration screen](/images/5-Workshop/5.5-Neon-Operations/image018.png)

![Figure 2. Login screen](/images/5-Workshop/5.5-Neon-Operations/image020.png)


#### Scenario 2. Browse POIs and Fetch Descriptions

| Field | Detail |
|-------|--------|
| **Objective** | Verify the ability to load the POI list and fetch description data from the backend |
| **Expected result** | Location data displays fully, images and descriptions load quickly with no API connection errors |

**Steps:**

1. On the home page, browse the POIs on the map.
2. Select a POI to view its detail.
3. Verify that the image, name, and description render correctly.

**Edge cases:**
- Case 1: POI disappears on page reload (rare — caused by cache not loading in time).

![Figure 3. Home screen with POI list](/images/5-Workshop/5.5-Neon-Operations/image022.png)


#### Scenario 3. Audio Playback via CloudFront

| Field | Detail |
|-------|--------|
| **Objective** | Ensure audio commentary files stream smoothly through the CloudFront CDN |
| **Expected result** | Audio plays fast, stable, no lag, and the correct file is served |

**Steps:**

1. On the home page, select a POI to listen to its commentary.
2. Press the **Play audio** button — audio starts automatically.
3. Verify that the pause/resume controls work correctly.

**Edge cases:**
- Case 1: File briefly freezes while the system is generating TTS (happens when the original audio file is deleted).
- Case 2: Wrong voice language served for a POI (already fixed).

![Figure 4. POI screen — audio playing](/images/5-Workshop/5.5-Neon-Operations/image024.png)
![Figure 5. POI screen — audio paused](/images/5-Workshop/5.5-Neon-Operations/image026.png)


#### Scenario 4. Tour Booking and Journey Tracking

| Field | Detail |
|-------|--------|
| **Objective** | Test the tour booking feature and journey status updates |
| **Expected result** | System automatically plays audio commentary and updates journey status as the user moves to each location in the tour |

**Steps:**

1. Go to the **Tour** page and select a tour (Premium Tours require unlocking).
2. Click **Start Journey**.
3. Move to the designated locations within the tour.
4. Verify that audio auto-plays and journey status updates correctly.

**Edge cases:**
- Case 1: Cannot start the tour because Premium Tours have not been unlocked.

![Figure 6. Locked Premium Tour screen](/images/5-Workshop/5.5-Neon-Operations/image028.png)

![Figure 7. Free Premium Tour screen](/images/5-Workshop/5.5-Neon-Operations/image030.png)

![Figure 8. Tour screen — commentary playing](/images/5-Workshop/5.5-Neon-Operations/image032.png)


#### Scenario 5. Payment Integration (Sandbox)

| Field | Detail |
|-------|--------|
| **Objective** | Test the payment gateway using a sandbox environment |
| **Expected result** | Payment succeeds and order status is updated correctly |

**Steps:**

1. Select a service or order to purchase.
2. Choose **PayPal** or a sandbox card as the payment method.
3. Fill in payment details and click **Confirm**.
4. Verify the order status updates to reflect the successful payment.

**Edge cases:**
- Case 1: Payment fails due to missing information.
- Case 2: Payment fails due to incorrect information.
- Case 3: Payment fails due to insufficient balance.
- Case 4: Payment fails because the device is offline.

![Figure 9. Payment screen — checkout opened](/images/5-Workshop/5.5-Neon-Operations/image034.png)

![Figure 10. Payment screen — PayPal selected](/images/5-Workshop/5.5-Neon-Operations/image036.png)

![Figure 11. Payment screen — card selected](/images/5-Workshop/5.5-Neon-Operations/image038.png)

![Figure 12. Payment screen — payment successful](/images/5-Workshop/5.5-Neon-Operations/image040.png)


#### Scenario 6. Error Handling (Invalid Data & Timeouts)

| Field | Detail |
|-------|--------|
| **Objective** | Ensure the exception-handling mechanism is user-friendly |
| **Expected result** | UI shows clear, actionable error messages without crashing the application |

**Steps:**

1. Enter invalid data into registration or booking forms.
2. Simulate network latency or timeout conditions.
3. Verify the error messages are clear and the app continues to function.


#### Scenario 7. Mobile Responsiveness

| Field | Detail |
|-------|--------|
| **Objective** | Ensure consistent behavior across Desktop and Mobile |
| **Expected result** | UI scales flexibly with no overflow or hidden critical elements |

**Steps:**

1. Use **Developer Tools** in the browser (or a physical device) to simulate various mobile screen sizes.
2. Verify UI and UX on both mobile and desktop viewports.

![Figure 13. Mobile interface](/images/5-Workshop/5.5-Neon-Operations/image042.png)
![Figure 14. Desktop interface](/images/5-Workshop/5.5-Neon-Operations/image044.png)


### 5.5.6.2. Testing Summary

The majority of test scenarios passed as expected. Identified edge cases have been documented, and most have either been resolved or have a fallback in place.

| Scenario | Result |
|----------|--------|
| Registration → Login | Passed |
| Browse POIs + Fetch descriptions | Passed |
| Audio playback via CloudFront | Passed |
| Tour booking + Journey tracking | Passed |
| Payment sandbox | Passed |
| Error handling & timeouts | Passed |
| Mobile responsiveness | Passed |
