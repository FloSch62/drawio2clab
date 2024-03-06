# Drawio2Clab

Drawio2Clab is a tool that converts .drawio diagrams to YAML format, specifically designed for network topologies. It parses XML files exported from draw.io (or diagrams.net), extracts information about nodes and links, and generates a structured YAML representation of the network. This process streamlines the setup of complex network topologies within [Containerlab](https://github.com/srl-labs/containerlab) environments, facilitating an easy and efficient way to manage container-based networking labs.

If you need the opposite, to generate a drawio file from a clab file, [containerlab2diagram](https://github.com/FloSch62/containerlab2diagram) has you covered.

## Features

- Converts .drawio diagrams to Containerlab-compatible YAML.
- Allows selection of specific diagrams within a .drawio file.
- Supports block and flow styles for YAML endpoints.
- Extracts detailed node and link information for precise topology representation.

## Drawing Constraints

When creating your .drawio diagrams, please adhere to the following constraints to ensure successful conversion:

- **Node Labeling**: All nodes must be labeled. To label a node, click on the node and start typing.
  
- **Link Labeling**: All links need to be labeled. To label a link, double-click on the link and type your label. Only the labels closest to the source and destination will be considered.
  
### Adding Node Data
In addition to labeling, nodes can contain additional data to further define the network configuration. The following attributes can be added to a node:

- `type`: Specify the type of the node. E.g., "ixrd2", "ixrd3".
- `kind`: Specify the kind of the node, by default nokia_srlinux
- `mgmt-ipv4`: Assign a management IPv4 address to the node.
- `group`: Define a group to which the node belongs.
- `labels`: Add custom labels for additional metadata or categorization.

To add these attributes to a node, select the node and add custom properties in the format `<property_name>=<value>`. Ensure each attribute is properly formatted according to the capabilities of draw.io for defining custom properties.

Those attributes will be added to the clab nodes.

![Drawio Example](img/drawio1.png)

The above image demonstrates how to correctly label nodes and links and add additional data to nodes for conversion.

## Running with Docker
To simplify dependency management and execution, Drawio2Clab can be run inside a Docker container. Follow these instructions to build and run the tool using Docker.

### Pulling from dockerhub
```bash
docker pull flosch62/drawio2clab:latest
```

### Building the Docker Image by yourself

Navigate to the project directory and run:

```bash
docker build -t drawio2clab .
```
This command builds the Docker image of Drawio2Clab with the tag drawio2clab, using the Dockerfile located in the docker/ directory.

### Running the Tool
Run Drawio2Clab within a Docker container by mounting the directory containing your .drawio files as a volume. Specify the input and output file paths relative to the mounted volume:
```bash
docker run -v "$(pwd)":/data drawio2clab -i /data/your_input_file.drawio -o /data/your_output_file.yaml
```
Replace your_input_file.drawio and your_output_file.yaml with the names of your actual files. This command mounts your current directory to /data inside the container.

## Requirements
- Python 3.6+

## Installation

### Virtual Environment Setup

It's recommended to use a virtual environment for Python projects. This isolates your project dependencies from the global Python environment. To set up and activate a virtual environment:

```bash
python3 -m venv venv
source venv/bin/activate  
```

### Installing Dependencies
After activating the virtual environment, install the required packages from the requirements.txt file:
```bash
pip install -r requirements.txt
```

## Usage
Convert a .drawio file to YAML:

```bash
python drawio2clab.py -i input_file.xml -o output_file.yaml
```

Specify a diagram name and output style:

```bash
python drawio2clab.py -i input_file.xml -o output_file.yaml --diagram-name "Diagram 1" --style flow
```

### Arguments
- -i, --input: Input .drawio XML file.
- -o, --output: Output YAML file.
- --style: YAML style (block or flow). Default is block.
- --diagram-name: Name of the diagram to parse.