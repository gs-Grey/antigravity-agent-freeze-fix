> 🇷🇺 Русская версия: [README.md](README.md)

## Fix for periodic Antigravity Agent freezes

Sometimes the Agent can get stuck indefinitely while executing commands. This can be fixed as follows:

1. **Switch your keyboard layout to English.**

2. **Open a new window or restart Antigravity.**

   > Important: the keyboard layout must be switched **before opening the window or launching Antigravity**.  
   > If you change the layout after the app is already running, the bug may not reproduce or be fixed.

3. **Paste the following command into the chat:**

```

Run Get-Date via run_command

```

   > It is important that the Agent runs *any* command specifically through `run_command`.

4. **If everything worked correctly**, a button will appear in the chat:  
**`Ran background terminal command → Open in Terminal`**  
Click it.

5. After that:

* you can use any language again,
* if any command freezes again —  
  simply **run it manually in the opened Antigravity Agent terminal**.

---

In simple terms:  
we initialize the terminal once via `run_command`, and after that we can always manually execute stuck commands through it.

---

## In short

The bug is not in the commands.  
The bug is that Antigravity prepends `cd`,  
and with a Russian keyboard layout it turns into `сcd`, which breaks the entire pipeline.

---

## More details on why this happens

Important: this bug depends on the **keyboard layout at the moment the Antigravity window/session is initialized**.  
The layout must be changed **before launching the app or opening a new window**,  
not after.

Antigravity has a built-in rule:  
**The Agent must work only inside the current project directory, to prevent accidental operations in other folders.**

This protection is implemented quite simply: before *every* command, the interpreter automatically prepends:

```

cd <project_path>

```

But there is a bug:  
if you have a **Russian keyboard layout** enabled, instead of `cd` it prepends:

```

сcd <project_path>

```

Where the first `с` is **Cyrillic**, not Latin.

As a result, the terminal receives a non-existent command `сcd`, breaks, and freezes.

It looks exactly like a human:

* started typing `cd`,
* realized the layout was Russian,
* switched to English,
* but forgot to delete the first Russian letter `с`.

---

## How to detect the bug (optional)

There are two simple ways to see this bug with your own eyes.

### 1. Via the Antigravity Agent terminal

If you already have the **Antigravity Agent terminal** open,  
during freezes you can often see something like this directly there:

```

сcd C:\path\to\project

```

or error messages related to `сcd`.

### 2. Via Developer Tools

If the **`Ran background terminal command → Open in Terminal`** button does not appear:

1. Press **F1** (or **Ctrl+Shift+P**).
2. Type: `Developer: Toggle Developer Tools`.
3. Select:  
   **“Developer: Toggle Developer Tools”**.
4. A separate window will open (like in Chrome).
5. Go to the **Console** tab and check if there are errors  
   (red text) at the moment when the command freezes.

This is where you can see that `сcd` is executed instead of `cd`.

### 3. Screenshots

![01](https://github.com/user-attachments/assets/08724d3c-ecba-4370-9f3d-51daf84fe37e)
![02](https://github.com/user-attachments/assets/1dc24719-f943-4b46-bd1d-7608d41b1bb9)
![03](https://github.com/user-attachments/assets/3000494c-5324-490a-bf48-3a54d8ae2bf2)
