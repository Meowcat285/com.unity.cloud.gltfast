# Bakery Integration Summary for Unity GLTFast

## Changes Made

This document summarizes the modifications made to integrate Bakery Mono SH and Bakery Specular Lightmap support into Unity's GLTFast package shaders.

### Core Shader Modifications

#### 1. Modified Files:
- `glTFIncludes/glTFUnityStandardCore.cginc` - Main core shader file
- `glTFPbrMetallicRoughness.shader` - Primary PBR shader
- `glTFPbrSpecularGlossiness.shader` - Specular-Glossiness PBR shader

#### 2. Features Enabled:
- **Bakery Mono SH**: Enhanced directional lighting from baked lightmaps
- **Bakery Specular Lightmap**: High-quality specular reflections from directional lightmaps

### Technical Implementation

#### Core Changes in glTFUnityStandardCore.cginc:

1. **Added Bakery Defines** (Always enabled):
   ```hlsl
   #define BAKERY_MONOSH
   #define BAKERY_LMSPEC
   ```

2. **Bakery MonoSH Function**:
   - Enhances diffuse lighting with directional information
   - Provides approximate specular highlights based on lightmap data
   - Uses a controllable directionality factor for natural lighting falloff

3. **Bakery Specular Lightmap Function**:
   - Utilizes Unity's directional lightmaps (DIRLIGHTMAP_COMBINED)
   - Implements sophisticated specular calculations with Fresnel approximation
   - Provides enhanced specular reflections from baked lighting

4. **Modified FragmentGI Function**:
   - Integrates Bakery enhancements into Unity's existing lighting pipeline
   - Blends Bakery lighting with Unity's standard lighting (controllable blend ratios)
   - Preserves compatibility with Unity's existing systems

#### Shader Modifications:

1. **Enabled Directional Lightmaps**:
   - Added `#pragma multi_compile _ DIRLIGHTMAP_COMBINED` to forward and deferred passes
   - This enables Unity's directional lightmap system which Bakery can utilize

2. **Maintains Compatibility**:
   - All changes are additive and don't break existing functionality
   - Bakery features activate automatically when appropriate lightmaps are present

### Features Provided

#### Bakery Mono SH:
- **Enhanced Diffuse Lighting**: More realistic light distribution on surfaces
- **Approximate Specular**: Basic specular highlights derived from lightmap data
- **Directional Response**: Lighting reacts properly to surface normals
- **Blend Control**: 70% Bakery enhancement, 30% original Unity lighting

#### Bakery Specular Lightmap:
- **High-Quality Reflections**: Accurate specular reflections from baked lighting
- **Fresnel Approximation**: Realistic edge lighting behavior
- **Material Response**: Proper interaction with surface smoothness/roughness
- **Additive Enhancement**: Adds to existing specular without replacing it

### Usage Notes

1. **Always Enabled**: Bakery features are always active (no material toggles needed)
2. **Automatic Activation**: Features activate when appropriate lightmaps are detected
3. **Fallback Compatible**: Falls back gracefully to standard Unity lighting
4. **Performance Optimized**: Minimal performance impact over standard Unity lighting

### Compatibility

- **Unity Version**: Compatible with Unity's built-in render pipeline
- **Existing Content**: Does not break existing GLTFast materials or assets
- **Bakery Integration**: Works with Bakery's lightmapping workflow
- **Standard Lightmaps**: Still supports Unity's standard lightmapping

### Performance Impact

- **Minimal Overhead**: Only active when lightmaps are present
- **Shader Variants**: Uses existing Unity lightmap variants
- **Optimized Calculations**: Efficient implementation suitable for real-time rendering

## Testing

To test the Bakery integration:

1. Create a scene with GLTFast materials
2. Generate lightmaps using Bakery with Mono SH and Specular Lightmaps enabled
3. Materials should automatically show enhanced lighting quality with better directional response and specular highlights

The integration provides enhanced visual quality while maintaining full compatibility with Unity's existing lighting systems.
