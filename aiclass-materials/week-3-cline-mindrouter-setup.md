# Install Cline and Configure a Custom Endpoint (MindRouter)

This guide covers installing the Cline extension in VS Code and pointing it at a
custom OpenAI-compatible inference endpoint (MindRouter). The same steps work for
any OpenAI-compatible provider.

---

## 1. Install Cline

1. Open VS Code.
2. Open the Extensions view: `Cmd+Shift+X` (macOS) or `Ctrl+Shift+X` (Windows/Linux).
3. Search for **Cline**.
4. Confirm the publisher is **Cline** (`saoudrizwan.claude-dev` is the extension ID, and has a blue verified badge). Several look-alike forks appear in the results, so pick the one that matches.
5. Click **Install**.
6. A robot icon appears in the Activity Bar on the left. Click it to open the Cline panel.

---

## 2. Confirm VPN Connectivity

> **Connect to the University of Idaho VPN first.** MindRouter is only reachable from the campus network. If you are off campus, start the U of I VPN (Cisco Secure Client / AnyConnect) and confirm it is connected. 

- If you don't have VPN access, please follow these self-service instruction: https://support.uidaho.edu/TDClient/40/Portal/KB/Article/3078/How-to-join-the-VPN-self-service-access-group
- Test access by using Vandal Chat: https://chat.uidaho.edu
  > Prompt: What is the weather in Moscow Idaho today


## 3. Get your MindRouter details ready

Follow the Quick Start guide: https://mindrouter.uidaho.edu

| Item | Value |
| --- | --- |
| Base URL | `https://mindrouter.uidaho.edu/v1` |
| API key | Starts with `mr2_` (you create this, see below) |
| Model name | A model id MindRouter serves, for example `default-llm-large` |

### Create an API key

Full MindRouter docs and Quick Start guide: <https://mindrouter.uidaho.edu/documentation.html>

1. With the VPN connected, open <https://mindrouter.uidaho.edu> in a browser.
2. Sign in with your University of Idaho account.
3. Follow the **Quick Start** guide in the docs above to create an API key.
4. Copy the key (it starts with `mr2_`) and keep it somewhere safe. You usually
   cannot view it again after leaving the page.

### Find a valid model name

Start with `default-llm-large`. It is the default large model on MindRouter
and a good, safe default for coding work. Enter it as the Model ID in the next
step. MindRouter offers many other models, so once things are working feel free
to try others and pick whichever you like best.

The rest of this section (2) is optional. If you are curious about other models MindRouter offers, here is how to look.

```bash
curl https://mindrouter.uidaho.edu/v1/models \
  -H "Authorization: Bearer mr2_your-api-key"
```

The Quick Start guide also lists the current models.

---

## 3. Configure the custom endpoint (Mindrouter) in Cline
This step connects VS Code to Mindrouter via the Cline plugin

1. Open the Cline panel (robot face icon in the Left side Activity Bar).
2. On first launch Cline asks **"How will you use Cline?"** Select the last
   option, **Bring my own API key** ("Use Cline with your provider of choice").
   The other options route you through Cline's own paid or hosted accounts,
   which we are not using.
   (If you do not see this screen, Cline is already set up. Click the
   **settings gear** at the top of the panel instead.)
3. Under **API Provider**, select **OpenAI Compatible**.
4. Fill in the fields:

   - **Base URL:** `https://mindrouter.uidaho.edu/v1`
     (enter it exactly like this, without `/chat/completions` on the end;
     Cline appends that itself)
   - **API Key:** `mr2_your-api-key`
   - **Model ID:** the model id you picked in step 2, for example `default-llm-large`

5. Leave other fields at their defaults unless you have a reason to change them:
   - **Context window size:** set this to match your model if Cline guesses wrong
     (for example `32768` or `131072`). Wrong values cause premature truncation
     or backend errors.
   - **Max output tokens:** `4096` is a safe starting point.
   - **Enable streaming:** on (default).

6. Click **Continue** / **Done** / **Save**.

### Ignore the ads

Cline shows promotional banners inside the panel, such as **"Try ClinePass"**
offering a paid monthly subscription. You do not need any of these. MindRouter is
your provider. Click the **X** in the
banner's top right corner to dismiss it and carry on.

### Optional: keep a separate profile for Claude

Cline supports multiple named API configurations:

1. In settings, use the **configuration profile** dropdown near the top.
2. Click **+** to add a profile. Name one `MindRouter` and one `Claude`.
3. Each profile stores its own provider, key, and model.
4. Switch profiles from that dropdown at any time. This lets you keep Anthropic
   (Claude) as one profile and MindRouter as another without re-entering settings.

---

## 4. First run

1. In the Cline panel, make sure the **MindRouter** profile is selected.
2. Type a small task, for example: `List the files in this folder and summarize the project.`
3. Cline proposes actions (reading files, running commands, editing files) and
   waits for your approval on each one.
4. Approve or reject each step. Use the **Plan / Act** toggle at the bottom of the
   input box to switch between "just discuss a plan" and "make changes".

### Auto-approve (optional, use with care)

The **Auto-approve** menu above the input box lets you pre-approve categories
(read files, edit files, run safe commands, use the browser). Start with only
**Read files** enabled until you trust the setup.

---

## 5. Troubleshooting

| Symptom | Likely cause and fix |
| --- | --- |
| `401 Unauthorized` | Wrong or missing API key. Re-check the `mr2_` key. Confirm the provider is **OpenAI Compatible**, not plain **OpenAI**. |
| `404 Not Found` | Base URL wrong. It must end in `/v1` and use your real deployment host. Do not include `/chat/completions`. |
| `model not found` | Model id does not match any healthy backend. Re-run the `/v1/models` curl and copy an exact id. |
| Responses cut off early | Context window size set too high or too low. Set it to the model's real limit. |
| Streaming errors or hangs | Toggle **Enable streaming** off and retry. Some backends behave better non-streamed. |
| Tool calls / function calling not working | The backend model may not support tool use. Pick a model that advertises tool / function calling, or expect Cline to fall back to plain text edits. |
| Nothing happens on send | Open **View -> Output**, choose **Cline** in the dropdown, and read the log. |
| Connection timeout / cannot reach host | You are off the University of Idaho VPN. Connect the U of I VPN and retry. |
| Cline panel / left sidebar disappeared | You toggled a VS Code view off. Press `Cmd+B` (macOS) or `Ctrl+B` (Windows/Linux) to bring the primary sidebar back. If Cline was docked on the right, use `Cmd+Option+B` (macOS) or `Ctrl+Alt+B` (Windows/Linux) for the secondary sidebar. |
| Terminal or Output pane disappeared | Press `Cmd+J` (macOS) or `Ctrl+J` (Windows/Linux) to toggle the bottom panel. |
| Panels still missing, or Cline's icon is gone from the Activity Bar | Open the Command Palette with `Cmd+Shift+P` (macOS) or `Ctrl+Shift+P` (Windows/Linux) and run **View: Reset View Locations**. This puts every view back where it started. Then right-click the Activity Bar and confirm **Cline** is checked. |

---

## 6. Reference

- MindRouter home: <https://mindrouter.uidaho.edu>
- MindRouter docs / Quick Start guide (where you create the API key): <https://mindrouter.uidaho.edu/documentation.html>
- Cline extension ID: `saoudrizwan.claude-dev`
- MindRouter OpenAI-compatible base URL: `https://mindrouter.uidaho.edu/v1`
- MindRouter models list: `GET https://mindrouter.uidaho.edu/v1/models`
- MindRouter also exposes an Anthropic-compatible route at
  `https://mindrouter.uidaho.edu/anthropic/v1/messages` if you later want to point
  Claude Code itself at it via `ANTHROPIC_BASE_URL`.
- Requires the University of Idaho VPN.
