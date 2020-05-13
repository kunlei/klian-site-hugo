---
title: Linux screen management
date: 2020-05-13
draft: false
---

Recently, I came across the task of testing the performance of an optimizer in a Linux environment and I wanted to test on multiple instances at the same time.
Essentially, what I need are two things:

- Able to run the optimizer for a long time without staying logged on.
- Able to run the optimizer with different inputs within the same login.

I was introduced the command `screen`and it solved my problem perfectly.
In a nutshell, a screen acts like a virtual environment and we can run all kinds of commands within it.
Logging out doesn't affect the screens that are running.

### Create a screen

To create a screen, just type the following command within the terminal:

```bash
screen
```

A new screen with a default id will be created and we're automatically located within this new screen.
Sometimes we want meaningful names for the screens we created, like in my case I wanted to run multiple instances corresponding to each screen.
To this purpose, all we need to do is to provide an additional parameter to this command:

```bash
screen -S screen-id
```

### Kill a screen

To terminate a screen that is no longer needed, press **ctrl + a**, followed by **K**.

### Detach and re-tach a screen

To detach from a screen, we just need to press **ctrl + a**, followed by **ctrl + d**.
To re-attach an existing screen, we need to run the command below:

```bash
screen -r screen-id
```

### List all screens

To list all the existing screens, if any, we can use:

```bash
screen -ls
```
