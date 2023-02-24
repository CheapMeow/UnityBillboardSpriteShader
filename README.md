# UnityBillboardSpriteShader

Billboard part is from:

https://en.wikibooks.org/wiki/Cg_Programming/Unity/Billboards

Unity billboard sprite shader, Modified from unity built-In sprite default Shader:

https://github.com/TwoTailsGames/Unity-Built-in-Shaders/blob/master/DefaultResourcesExtra/Sprites-Default.shader

https://github.com/TwoTailsGames/Unity-Built-in-Shaders/blob/master/CGIncludes/UnitySprites.cginc

May help when you use your custom billboard shader and find some sprites appear with unexpected white outline.

Related problem in forum: 

https://answers.unity.com/questions/930472/unable-to-remove-white-outline-for-billboards.html

The core part is in BillboardSprites.cginc

```shaderlab
 inline float4 Billboard(float4 vertex)
 {
     return mul(UNITY_MATRIX_P,
         mul(UNITY_MATRIX_MV, float4(0.0, 0.0, 0.0, 1.0))
         + float4(vertex.x, vertex.y, 0.0, 0.0)
         * float4(_ScaleX, _ScaleY, 1.0, 1.0));
 }
 
 v2f SpriteVert(appdata_t IN)
 {
     v2f OUT;
 
     UNITY_SETUP_INSTANCE_ID(IN);
     UNITY_INITIALIZE_VERTEX_OUTPUT_STEREO(OUT);
 
     OUT.vertex = UnityFlipSprite(IN.vertex, _Flip);
     OUT.vertex = Billboard(OUT.vertex);
     OUT.texcoord = IN.texcoord;
     OUT.color = IN.color * _Color * _RendererColor;
 
 #ifdef PIXELSNAP_ON
     OUT.vertex = UnityPixelSnap(OUT.vertex);
 #endif
 
     return OUT;
 }
 ```
