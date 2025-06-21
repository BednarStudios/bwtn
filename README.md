BWnT - Bednar World to Native Translator

BWnT (Bednar World to Native Translator) is a Python CLI tool for converting Minecraft Java Edition region files (.mca) into a compact binary format (.bmap) suitable for use with the Bednar voxel engine in Godot.


---

🧠 Purpose

Minecraft region files store terrain in a format optimized for Minecraft itself. BWnT extracts only the relevant data (block ID, position, rotation) and stores it efficiently for fast loading in custom voxel engines.

This allows Bednar (or similar Godot voxel games) to use Minecraft worlds without relying on slow or bloated runtime parsers.


---

📦 Features

✅ Converts .mca files to .bmap format

✅ Skips air blocks for efficiency

✅ Optimized binary output (8 bytes per block)

✅ Clean CLI interface

✅ Logging and progress display

✅ Easily extensible for block metadata



---

📥 Installation

🔹 Install from PyPI:

pip install bwtn

This will install the CLI tool bwtn globally.

🔹 Development version:

git clone https://github.com/yourusername/bwtn.git
cd bwtn
pip install .


---

⚙️ Usage

🔹 Command-line usage:

bwtn convert --mcworldpath <path-to-region> --destinationpath <output-folder> --worldname <name> [--verbose]

🔹 Example:

bwtn convert \
  --mcworldpath Deadwood/region/r.0.0.mca \
  --destinationpath ./Deadwood_converted \
  --worldname "Deadwood" \
  --verbose

🔹 Flags:

Flag	Description

--mcworldpath	Path to the .mca file
--destinationpath	Output directory
--worldname	Name of the world to write in metadata
--verbose	Enable detailed logs and progress display



---

📁 Output Structure

Deadwood_converted/
├── level_world.properties   # UTF-8 encoded world name
└── db/
    └── region.bmap          # Binary format file with all blocks


---

📄 Binary Format (.bmap)

Each block is stored as 8 bytes in little-endian format:

Field	Type	Size	Description

block_id	uint16	2 B	ID of the block
x	int16	2 B	Global X position
y	uint8	1 B	Y position (0-255)
z	int16	2 B	Global Z position
rotation	uint8	1 B	Rotation (optional)


Only blocks with block_id != 0 are written.


---

💡 How to Use in Godot

In Godot (GDScript):

var world_blocks = []
var file = FileAccess.open("res://Deadwood_converted/db/region.bmap", FileAccess.READ)

while not file.eof_reached():
    var block_id = file.get_16()
    var x = file.get_16()
    var y = file.get_8()
    var z = file.get_16()
    var rot = file.get_8()
    world_blocks.append({
        "block_id": block_id,
        "position": Vector3i(x, y, z),
        "rotation": rot
    })

file.close()


---

🛠 Development

To modify and test locally:

git clone https://github.com/yourusername/bwtn.git
cd bwtn
pip install -e .

You can now test the command via:

python -m bwtn.cli convert --help


---

📢 Contributing

Feel free to fork the project and submit PRs! If you want to improve block metadata handling, add support for biomes, or multiple regions, open an issue first to discuss.


---

📜 License

This project is licensed under the MIT License.


---

📚 Credits

anvil-parser2 for MCA parsing

Original anvil-parser by matcool

Special thanks to the Bednar Engine dev team 🚀



---

🔗 Links

GitHub Repository

PyPI Package

Godot Engine
