Line 42 - Initialization of RawShaderMaterial

Line 71 - Beginning of Vertex Shader
<script id="vertexShader" type="x-shader/x-vertex">
  precision mediump float;

  attribute vec3 position;
  attribute vec3 normal;
  attribute vec2 uv;

  uniform mat4 modelViewMatrix;
  uniform mat4 projectionMatrix;
  uniform mat4 normalMatrix;

  varying vec3 vNormal;
  varying vec4 vPosition;
  varying vec2 vUV;

  void main()
  {
    vUV = uv;

    vNormal = vec3(normalMatrix * vec4(normal, 0.0));

    vPosition = modelViewMatrix * vec4(position, 1.0);

    gl_Position = projectionMatrix * vPosition;
  }
</script>

Line 98 - Beginning of Fragment Shader
<script id="fragmentShader" type="x-shader/x-fragment">
  precision mediump float;

  uniform vec3 ambientColor;
  uniform sampler2D map;
  uniform vec3 lightPos;
  uniform mat4 viewMatrix;

  varying vec3 vNormal;
  varying vec4 vPosition;
  varying vec2 vUV;

  void main()
  {
    vec3 updatedLight = vec3(viewMatrix * vec4(lightPos, 1.0));
    vec3 L = normalize(lightPos - vPosition.xyz);
    float lambert = clamp(dot(vNormal, L), 0.0, 1.0);

    gl_FragColor = vec4(sin(lambert) * texture2D(map, vUV).xyz, 1.0);
  }
</script>
