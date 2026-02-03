---
name: fast-android-build
description: Use when the user wants to run Android/Gradle builds faster, or when a "debug build" is requested. Covers parallelism, build cache, configuration cache, and offline mode optimizations.
---

# Fast Android Build

## Overview

This skill provides highly optimized Gradle commands to speed up the build process of Android applications, specifically tailored for the "Connection" project.

## Core Principles

1. **Parallelism**: Use all available CPU cores.
2. **Caching**: Reuse previous build results whenever possible.
3. **Configuration Cache**: Skip the configuration phase for subsequent builds.
4. **Offline Mode**: Avoid network checks if dependencies are already downloaded.

## Optimized Commands

### Standard Debug Build

Use this for the fastest day-to-day development build:

```powershell
./gradlew assembleDebug --parallel --build-cache --configuration-cache --no-daemon
```

### Build with Apps and Tests

Use this when you need to verify changes with both build and unit tests:

```powershell
./gradlew assembleDebug testDebugUnitTest --parallel --build-cache --configuration-cache --no-daemon
```

### Fast Unit Tests Only

Use this for the quickest possible test execution cycles:

```powershell
./gradlew testDebugUnitTest --parallel --build-cache --configuration-cache --no-daemon --offline
```

### Clean Build (The Nuclear Option)

Only use this if you encounter weird caching issues:

```powershell
./gradlew clean assembleDebug --parallel --build-cache --configuration-cache --no-daemon
```

## Flags Explained

- `--parallel`: Executes tasks in parallel where possible.
- `--build-cache`: Reuses outputs from previous builds, even across different branches.
- `--configuration-cache`: Caches the result of the configuration phase.
- `--no-daemon`: In agentic environments, the daemon can sometimes cause file lock issues; we usually prefer explicit execution.
- `-Pkotlin.incremental=true`: Ensures Kotlin incremental compilation is active.

## When to Use

- When the user asks for a "debug build".
- When you need to "compile the app".
- When building takes more than 1 minute and you haven't optimized flags yet.

> [!TIP]
> If you are on a slow internet connection, add `--offline` to the commands to prevent Gradle from checking for dependency updates.
