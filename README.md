# Gamino

Gamino is a minimal F# MonoGame project demonstrating how to set up a 3D isometric rendering environment using `.obj` models. It features an "assembled" character composed of multiple 3D parts (head, body, arms) rendering in a staggered isometric projection.

> Assets used in this project are sourced from https://kaylousberg.itch.io/prototype-bits

## Key Features

- **F# Architecture**: Uses a functional, module-based architecture separating Game Logic, Initialization, and Rendering from the MonoGame `Game` class wrapper.
- **Isometric Camera**: Implements an orthographic camera setup to simulate a 2.5D isometric view suitable for staggered tile maps.
- **3D Model Compositing**: Demonstrates how to render multiple independent 3D model parts sharing a common origin to appear as a single animated entity.

## Project Structure

- `Program.fs`: The main entry point and game logic.
- `Gamino.Core`: A module containing the functional core of the game.
- `Content/`: Contains the assets and the MonoGame Content Builder configuration (`.mgcb`).

## Working with Content (MonoGame Content Pipeline)

This project uses the `dotnet-mgcb` tool to compile assets (models, textures, etc.) into the binary format MonoGame uses at runtime (`.xnb`).

### Prerequisites

Ensure you have the local tool installed (it is configured in the `.config/dotnet-tools.json` manifest):

```bash
dotnet tool restore
```

### Building Content

The content is automatically built when you run `dotnet build`. However, you can trigger a content build manually:

```bash
dotnet mgcb-editor-linux Content/Content.mgcb
```

_(Note: Replace `dotnet-mgcb-editor-linux` with the appropriate command for your OS if different, or use `dotnet mgcb` for the CLI version)._

### Important: The `.obj` File Caveat

When importing Wavefront **.obj** files in this project, we encountered a specific issue with the default `OpenAssetImporter`. It sometimes fails to process materials correctly for simple `.obj` files, leading to runtime crashes (`ContentLoadException` regarding `ReflectiveReader`).

**The Fix:**
We explicitly force the Content Pipeline to use the **`FbxImporter`** for `.obj` files in `Content/Content.mgcb`, as it handles the material mapping more reliably for these assets.

**Example `.mgcb` entry:**

```ini
#begin Assets/obj/MyModel.obj
/importer:FbxImporter  <-- CRITICAL: Override default importer
/processor:ModelProcessor
/processorParam:TextureFormat=Compressed
/build:Assets/obj/MyModel.obj
```

## Running the Game

Simply run the project from the terminal:

```bash
dotnet run
```
