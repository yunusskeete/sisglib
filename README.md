# sisglib
Spatial Intelligence Scene Generation Library

**Spatial Intelligence Scene Generation Library**
A unified toolkit for developing, evaluating, and deploying 3D scene generation algorithms - built for research and production.

**For researchers:** The *OpenAI Gym* of 3D scene generation - a common framework for implementing, testing, and benchmarking novel approaches.

**For engineers:** The *Hugging Face Transformers* of 3D scene generation - a modular foundation for building, integrating, and deploying systems in real-world pipelines.

## Quickstart:
- Playable and editable 3D scenes for Blender, Unity, Unreal, etc. with a simple, modular API.
- Modular and composable asset storage, metadata, and embeddings (vectors) backends for plug-and-play integration with any service.

Demo end-to-end scene generation:
```python
from sisglib.adapters import MetadataAdapter, StorageAdapter, VectorsAdapter
from sisglib.pipeline import SceneGenerationPipeline
from sisglib.visualization import SceneVisualizer

from .architecture import ArchitectureGenerator
from .layout import LayoutGenerator
from .solver import LayoutSolver


def demo_end_to_end_scene_generation() -> None:
    """
    Demonstrates an end-to-end 3D scene generation workflow.

    Flow:
    1. Generates an architectural layout from a text prompt.
    2. Retrieves and places objects within the scene.
    3. Produces a final scene specification.
    4. Optionally visualizes and/or exports the scene.
    """
    from sisglib.config import load_config

    # --- 1. Define the user input ---
    prompt: str = "A small modern apartment with living room, kitchen, and bedroom."

    # --- 2. Initialise adapters (pluggable backends) ---
    config = load_config("config.yaml")
    vectors_adapter = VectorsAdapter.from_config(config["vectors"])
    metadata_adapter = MetadataAdapter.from_config(config["metadata"])
    storage_adapter = StorageAdapter.from_config(config["storage"])

    # --- 3. Build pipeline with modular components ---
    pipeline = SceneGenerationPipeline(
        vectors_adapter=vectors_adapter,
        metadata_adapter=metadata_adapter,
        storage_adapter=storage_adapter,
        strategy="defualt",
        llm_backend="openai:gpt-4",
        solver_backend="default-physics",
    )

    # --- 4. Run the end-to-end generation process ---
    architecture_spec = pipeline.generate_architecture_spec(prompt)
    print("\n[ARCHITECTURE SPEC]")
    print(architecture_spec)

    object_specs = pipeline.generate_object_specs(architecture_spec)
    print("\n[OBJECT SPECS]")
    print(object_specs)

    scene_spec = pipeline.compose_scene(
        architecture_spec=architecture_spec, object_specs=object_specs
    )
    print("\n[SCENE SPEC]")
    print(scene_spec)

    # --- 5. Optional visualization step ---
    visualizer = SceneVisualizer(engine="blender")
    visualizer.preview(scene_spec)
    print(
        "\n✅ Scene generation complete! Exported to:",
        storage_adapter.output_path(scene_spec),
    )


if __name__ == "__main__":
    demo_end_to_end_scene_generation()
```

## Release:
Coming soon at [3D-Intelligence/sisglib](https://github.com/3D-Intelligence/sisglib)

## Contact:
[Me!](https://github.com/yunusskeete)
