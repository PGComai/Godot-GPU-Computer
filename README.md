# Godot-GPU-Computer
A helper class for using compute shaders in Godot 4.

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/Y8Y0NJZLS)

## Example
```GDScript
## create new GPUComputer instance
var gc := GPUComputer.new()

# Called when the node enters the scene tree for the first time.
func _ready():
	## set shader file path
	gc.shader_file = load("res://shader.glsl")
	## initialize shader
	gc._load_shader()
	
	## create data array and convert to bytes
	var example_array := PackedFloat32Array([1.0, 2.0, 3.0, 4.0, 5.0])
	var example_bytes := example_array.to_byte_array()
	
	## create a buffer from the byte array
	gc._add_buffer(0, 0, example_bytes)
	
	## create uniforms from buffers, make render pipeline, set workgroup size
	gc._make_pipeline(Vector3i(5, 1, 1), true)
	
	## send to GPU and then request return data
	gc._submit()
	gc._sync()
	
	## get buffer output
	var example_output_bytes := gc.output(0, 0)
	var example_output := example_output_bytes.to_float32_array()
	
	## if you wish to modify the buffer here, you may do so and update it
	## otherwise this is unnecessary
	gc._update_buffer(example_output_bytes, 0, 0)
	
	gc._make_pipeline(Vector3i(5, 1, 1))
	
	gc._submit()
	gc._sync()
```
