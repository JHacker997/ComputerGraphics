Line 51 - Instantiate raw shader
Line 80 - Begin vertex shader
Line 108 - Begin fragment shader

<script id="vertexShader" type="x-shader/x-vertex">
  precision mediump float;

  attribute vec3 position;
  attribute vec3 normal;
  attribute vec2 uv;

  uniform mat4 modelMatrix;
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

<script id="fragmentShader" type="x-shader/x-fragment">
  precision mediump float;

  uniform sampler2D map;
  uniform vec3 lightPosition;
  uniform mat4 viewMatrix;
  uniform mat4 modelMatrix;
  uniform mat4 modelViewMatrix;
  uniform samplerCube cubeMap;

  varying vec3 vNormal;
  varying vec4 vPosition;
  varying vec2 vUV;

  void main()
  {
    vec3 updatedLight = vec3(modelMatrix * vec4(lightPosition, 1.0));
    vec3 L = normalize(updatedLight - vPosition.xyz);
    vec3 I = normalize(vPosition.xyz - updatedLight);
    vec3 N = normalize(vNormal);
    float NdotL = clamp(dot(N,L), 1.0, 1.0);
    vec3 R = reflect(I, N);
    mat3 vM = mat3(viewMatrix);
    vec3 R_in_worldspace = vec3(dot(vM[0],R),dot(vM[1],R),dot(vM[2],R));
    vec4 envMapColor = textureCube(cubeMap, R_in_worldspace);

    gl_FragColor = vec4(envMapColor) * NdotL;
  }
</script>
