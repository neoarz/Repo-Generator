
# Neo's App Repository Generator

A cross-platform command-line utility for generating SideStore-compatible JSON repositories from IPA files.

## Overview

This tool scans IPA files, extracts app metadata and icons, and generates a properly formatted repository JSON file for use with SideStore and similar iOS sideloading applications.

## Platform Support

- **macOS** (`macos.py`): Best icon extraction using native tools (`xcrun`, `assetutil`, and `sips`)
- **Windows** (`windows.py`): Basic icon extraction from IPA structure
- **Linux** (`linux.py`): Similar capabilities to Windows version

## Requirements

- Python 3.6 or higher
- PIL (Python Imaging Library): `pip install pillow`

## Usage

Basic command to process IPA files:

```bash
# Use the appropriate script for your platform
python macos.py --ipa path/to/app.ipa --output my_repo.json

# OR process all IPAs in a folder
python windows.py --folder path/to/ipa_folder --output my_repo.json

# WITH custom repository details
python linux.py --folder path/to/ipa_folder \
  --output my_repo.json \
  --name "My Custom Repository" \
  --id "com.example.repository" \
  --icon-dir "custom_icons_folder"
```

## Command Line Arguments

| Argument | Description | Default |
|----------|-------------|---------|
| `--ipa` | Path to one or more IPA files | [] |
| `--folder` | Folder containing IPA files (will be searched recursively) | None |
| `--output` | Output JSON file | apps.json |
| `--name` | Repository name | Neo's App Repository |
| `--id` | Repository identifier | com.neo.repository |
| `--icon-dir` | Directory for storing extracted app icons | app_icons |

## Output

The tool generates:

1. A JSON repository file with app metadata
2. A directory containing extracted app icons

### Example Repository JSON (Simplified)

```json
{
  "name": "Neo's App Repository",
  "identifier": "com.neo.repository",
  "sourceURL": "https://example.com/com.neo.repository.json",
  "iconURL": "https://example.com/repo-icon.png",
  "userinfo": {},
  "apps": [
    {
      "name": "Example App",
      "bundleIdentifier": "com.example.app",
      "version": "1.2.3",
      "absoluteVersion": "123",
      "versionDate": "2025-03-09T10:00:00Z",
      "size": 15000000,
      "developerName": "Unknown Developer",
      "iconURL": "Example_App.png",
      "tintColor": "000000",
      "subtitle": "Example App for iOS",
      "localizedDescription": "App extracted from IPA file: Example_App.ipa",
      "versionDescription": "Version 1.2.3",
      "downloadURL": "https://example.com/downloads/Example_App.ipa",
      "screenshotURLs": [],
      "appID": "com.example.app",
      "versions": [
        {
          "version": "1.2.3",
          "date": "2025-03-09T10:00:00Z",
          "downloadURL": "https://example.com/downloads/Example_App.ipa",
          "localizedDescription": "Version 1.2.3",
          "size": 15000000,
          "absoluteVersion": "123"
        }
      ]
    },
```

## How It Works

1. Finds IPA files in specified locations
2. Extracts each IPA to a temporary directory
3. Reads app metadata from Info.plist
4. Extracts app icons using platform-specific strategies
5. Generates structured repository JSON
6. Saves extracted icons to the specified directory
