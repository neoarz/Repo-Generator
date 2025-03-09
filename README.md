# Neo's App Repository Generator

A command-line utility for generating SideStore-compatible JSON repositories from IPA files on macOS, Windows, and Linux.

## Overview

This tool allows you to create app repositories by scanning IPA files, extracting app metadata and icons, and generating a properly formatted repository JSON file for use with SideStore and similar sideloading applications.

## Requirements

### macOS
- Python 3.6 or higher
- PIL (Python Imaging Library)
- macOS built-in tools (`xcrun`, `assetutil`, and `sips`)

### Windows
- Python 3.6 or higher
- PIL (Python Imaging Library)

### Linux
- Python 3.6 or higher
- PIL (Python Imaging Library)

## Installation

1. Clone or download this repository
2. Install required dependencies:
   ```
   pip install pillow
   ```

## Usage

### Basic Usage

Process a single IPA file:

```bash
python app_repo_generator.py --ipa path/to/app.ipa --output my_repo.json
```

Process multiple IPA files:

```bash
python app_repo_generator.py --ipa path/to/app1.ipa path/to/app2.ipa --output my_repo.json
```

Process all IPAs in a folder (including subfolders):

```bash
python app_repo_generator.py --folder path/to/ipa_folder --output my_repo.json
```

### Advanced Options

You can customize the repository with the following options:

```bash
python app_repo_generator.py --folder path/to/ipa_folder \
  --output my_repo.json \
  --name "My Custom Repository" \
  --id "com.example.repository" \
  --icon-dir "custom_icons_folder"
```

### Command Line Arguments

| Argument | Description | Default |
|----------|-------------|---------|
| `--ipa` | Path to one or more IPA files | [] |
| `--folder` | Folder containing IPA files (will be searched recursively) | None |
| `--output` | Output JSON file | apps.json |
| `--name` | Repository name | Neo's App Repository |
| `--id` | Repository identifier | com.neo.repository |
| `--icon-dir` | Directory for storing extracted app icons | app_icons |

## How It Works

The tool performs the following steps:

1. Searches for IPA files in the specified locations
2. Extracts each IPA to a temporary directory
3. Reads the app's Info.plist to get metadata (name, bundle ID, version)
4. Extracts app icons using multiple strategies
   - On macOS: Uses `assetutil` or `actool` for Assets.car extraction
   - On all platforms: Finds icon files referenced in Info.plist and common naming patterns
5. Combines all app data into a structured repository JSON
6. Saves extracted icons to the specified icon directory

## Platform-specific Notes

### macOS
The macOS version offers the most complete icon extraction capabilities, using native tools like `assetutil`, `actool`, and `sips` to extract high-quality icons from Assets.car files.

### Windows and Linux
The Windows and Linux versions rely on more basic icon extraction methods, primarily extracting icons directly from the IPA file structure based on filenames referenced in the Info.plist. Icon extraction may be less reliable compared to macOS.

## Output

The tool generates:

1. A JSON repository file with app metadata
2. A directory containing extracted app icons

### Repository JSON Structure

The generated repository includes the following information:

- Repository metadata (name, identifier, sourceURL)
- List of apps, each containing:
  - App name, bundle ID, version
  - Icon URL
  - Download URL
  - Version history
  - Other metadata

### Example Repository JSON

Here's an example of the generated repository JSON:

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
    {
      "name": "Another App",
      "bundleIdentifier": "com.another.app",
      "version": "2.0.1",
      "absoluteVersion": "201",
      "versionDate": "2025-03-09T10:00:00Z",
      "size": 25000000,
      "developerName": "Unknown Developer",
      "iconURL": "Another_App.png",
      "tintColor": "000000",
      "subtitle": "Another App for iOS",
      "localizedDescription": "App extracted from IPA file: Another_App.ipa",
      "versionDescription": "Version 2.0.1",
      "downloadURL": "https://example.com/downloads/Another_App.ipa",
      "screenshotURLs": [],
      "appID": "com.another.app",
      "versions": [
        {
          "version": "2.0.1",
          "date": "2025-03-09T10:00:00Z",
          "downloadURL": "https://example.com/downloads/Another_App.ipa",
          "localizedDescription": "Version 2.0.1",
          "size": 25000000,
          "absoluteVersion": "201"
        }
      ]
    }
  ]
}
```

## Examples

### Creating a Basic Repository

```bash
python app_repo_generator.py --folder ~/Downloads/MyApps --output my_repo.json
```

### Creating a Repository for a Single App

```bash
python app_repo_generator.py --ipa ~/Downloads/MyApp.ipa --output single_app_repo.json
```

### Creating a Custom Named Repository

```bash
python app_repo_generator.py --folder ~/Downloads/MyApps \
  --output custom_repo.json \
  --name "My Personal App Collection" \
  --id "com.mysite.appcollection"
```


## License
