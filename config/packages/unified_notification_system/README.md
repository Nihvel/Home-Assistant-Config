# Unified Notification System

The `Unified Notification System` or `uns` is a custom script that allows you to send messages to various devices in your Home Assistant setup. 
The messages can be sent as **text** or **voice** and are fully customizable. 

---

## **Features:**

- **Send Messages**: You can send messages in **text** or **voice** format.
- **Customizable Parameters**: Control which devices receive the message, the volume for voice messages, and much more.
- **Flexible and Dynamic**: Easily integrate into your Home Assistant setup to automate notifications.

---

## **How It Works**

### **The Basic Structure**

```yaml
- action: script.uns
  data:
    kind: voice | text         [!]   # Type of message: either voice or text
    message_id: {macro_name}   [X]   # Unique identifier for the message (choose one)
    message: {message}         [X]   # The actual message content (choose one)
    forced: True | False       [opt][kind=voice]   # Whether to force the message (optional for voice)
    engine: {engine}           [opt][kind=voice]   # TTS engine to use (optional for voice)
    player: {player_name}      [opt][kind=voice]   # Name of the player (optional for voice)
    volume: {volume}           [opt][kind=voice]   # Volume of the playback (optional for voice)
    device: {device} | all     [opt][kind=text]    # Device to send the message to (optional for text)
```

---

### **Parameters Explained**

1. **`kind: voice | text`**  
   Specifies whether you want to send a **voice** message (spoken aloud) or a **text** message (written).  
   **Required** field.

2. **`message_id: {macro_name}`**  
   A unique identifier for the message. You can use a macro name to dynamically assign it.  
   **Mutually Exclusive** with the `message` parameter—**only one** of these should be used.

3. **`message: {message}`**  
   The actual message content that you want to send (e.g., "Hello, how are you?").  
   **Mutually Exclusive** with `message_id`.

4. **`forced: True | False`**  
   If set to `True`, the message will be forced, meaning it will interrupt other audio or messages.  
   Only relevant when **sending a voice message**.

5. **`engine: {engine}`**  
   Specifies which **Text-to-Speech (TTS)** or **LLM** engine to use for voice messages. You might use engines like Google TTS, Home Assistant Cloud, Piper, OpenAI (LLM) and more.  
   Optional, but only for **voice messages**.

6. **`player: {player_name}`**  
   The **name of the player** (e.g., your speaker or media player) that will play the voice message.  
   Optional, but only for **voice messages**.

7. **`volume: {volume}`**  
   Set the **volume level** for the player (e.g., `50` for 50% volume).  
   Optional, but only for **voice messages**.

8. **`device: {device} | all`**  
   Specifies the **device** that will receive the text message. You can use `all` to send it to every device.  
   Optional, but only for **text messages**.

---

## **How to Use It**

To use this script in your automation, you can place it within a YAML file in your Home Assistant setup. For example:

### **Example 1: Voice Message to Player**

```yaml
- action: script.uns
  data:
    kind: voice
    message: Hello, this is a test message!
    player: living_room
    volume: 75
```

This example sends a voice message to the **Living Room Speaker** with a **volume of 75%**.

### **Example 2: Text Message to all devices**

```yaml
- action: script.uns
  data:
    kind: text
    message_id: call_door_open
```

This sends a **preconfigured text message** to all devices.

### **Example 3: Using a TTS Engine for Voice Message**

```yaml
- action: script.uns
  data:
    kind: voice
    message: Dinner is ready!
    player: office
    engine: google
```

This will send a **voice message** using the **Google TTS engine** to the **Office Speaker**. 
If the speaker is currently playing something it will pause the media, announce the message and then resume playing.

### **Example 4: Sending a critical voice message over all players**

```yaml
- action: script.uns
  data:
    kind: voice
      message_id: call_water_leak
      forced: true
      player: all
```

This will send a **preconfigured emergency voice message** using the default engine on all the players. The `forced: true` will play no matter the time of the day or **quiet_mode** (speech_engine: false)


### **Example 5: Using an LLM**

```yaml
- action: script.uns
  data:
    kind: voice
      message: Tell me a joke
      engine: openai_35
```

In this example, we're using an LLM (Large Language Model) as the engine for generating the message. The engine parameter here is set to openai_35, which refers to the name of the OpenAI model you setup using the OpenAI integration.

When you choose an LLM as the engine, it doesn't directly produce a voice message. Instead, the LLM generates the content. 
Once the message is created, you can further configure the script to pass that content to a Text-to-Speech (TTS) service that converts the text into spoken output.

---

## **Important Notes:**

- **Mutually Exclusive Parameters**: You can only use **either** `message_id` **or** `message` in each action—**not both**. Choose one depending on whether you need a dynamic identifier or a static message. You can pass a `message_id` to an LLM and concatenate multiple `macro` together!
- **Optional Parameters**: The fields like `forced`, `engine`, `player`, and `volume` are optional and only apply if you're sending a **voice message**. Similarly, `device` is optional for **text messages** because it defaults to `all`.

---

## **Known BUGS:**

- **Multiple players**: You can play a voice message on multiplayer players with `player: all` but if anything is playing already on any of the players this won't resume what you were currently playing.
---

## **Conclusion**

This script gives you the flexibility to send dynamic, automated messages, either as text or voice, to your devices in Home Assistant. 
Whether you want to notify family members or play an announcement aloud, this system is highly customizable and easy to integrate into your setup.

If you have any questions or need further assistance, feel free to open an issue or reach out!

---

### **License**

This project is licensed under the MIT License.

---

### **Author**

The initial version of this project, back in 2018, was created only for myself @Nihvel when I started to get coinfident with Home Assistant and I started reading how other people were configuring their own HA, in particular, [@CCOSTAN's HA config](https://github.com/CCOSTAN/Home-AssistantConfig).

Today I decided to share it with the community because it may help YOU to build your own personal Voice Pipeline for Home Assistant!

---
