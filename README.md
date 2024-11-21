<p align="center"><img src="./ZKP_logo.png" alt="ZKP Logo" width="200" height="auto"></p>

# ZKP-Data-Quality

The power of Zero-Knowledge Proofs (ZKPs) unlocks a wide range of compelling use cases.

ZKP-Data-Quality harnesses the transformative power of Zero-Knowledge Proofs (ZKPs) to deliver trustless verification of dataset quality. This allows a prover to demonstrate that a dataset, at a specific point in time, meets the qualitative criteria required by the verifier—all without exposing the underlying data. This ensures both data integrity and privacy.  

The ability to validate constraints without revealing sensitive information revolutionizes collaborative processes, whether within organizations or in dealings with external partners. It enables secure, reliable, and swift data sharing, unlocking new opportunities, accelerating innovation cycles, and driving greater business value.  

Moreover, regulatory compliance becomes significantly more streamlined. For instance, datasets in medical trials or financial reporting can be verified to meet predefined quality thresholds at the time of use, ensuring adherence to strict regulatory standards while preserving data confidentiality.  

## Project Overview

The ZKP-Data-Quality suite of proofs is designed to achieve two critical objectives:

### 1. Proof of Provenance  
How can we ensure that any claims made about a dataset are genuinely tied to that specific dataset?
This is achieved by embedding the root hash of all dataset hash values directly into the proof. For enhanced efficiency within the ZKP circuit, the proof also includes the root hash of the dataset's sorted hashes. By incorporating both the sorted and unsorted root hashes, the proof can be traced back to the original dataset, ensuring complete traceability. This approach eliminates the risk of data quality claims being misattributed to an incorrect dataset.

### 2. Proof of Quality Aspects   
Once the dataset's provenance is established, qualitative attributes can be validated without exposing the underlying data. For instance, proofs can be generated to demonstrate aspects like row uniqueness or row completeness, even for dynamic, 'live' datasets. These proofs can also be timestamped to provide a verifiable snapshot of data quality at a specific moment in time.  

Note: Refer to the [ISO](#iso-standards) section for additional context on data quality standards.

## Features

- **Data Hashing:** Utilizes cryptographic hash functions to represent data entries securely.
- **Merkle Tree Construction:** Builds Merkle Trees for both original and sorted datasets to facilitate efficient and secure data verification.
- **Zero-Knowledge Proof Generation:** Generates proofs to verify data properties without revealing the data itself.
- **Proof Verification:** Verifies the generated proofs to ensure data integrity and correctness.

## Installation

You can set up the project natively or more easily using docker.

### 1. Native Installation
To run the project natively without Docker, follow these steps to install the necessary software dependencies:

- Node.js + npm
- Rust and Cargo 
- Circom

The project package.json file contains the required npm installed dependencies.
```json
  "dependencies": {
    "circomlib": "^2.0.5",
    "circomlibjs": "^0.1.7",
    "crypto": "^1.0.1",
    "crypto-browserify": "^3.12.1",
    "csv-parser": "^3.0.0",
    "snarkjs": "^0.7.4"
  }
```

1. **Install Node.js and npm:**
   
   Ensure that Node.js (version 20 or higher) and npm are installed on your system.  
   You can download them from the [official Node.js website](https://nodejs.org/).
   
   To verify the installation, run:
   ```bash
   node -v
   npm -v
   ```

1. **Install Python 3 and Pip:**

   Some scripts may require Python 3. Install Python 3 and its package manager, pip, from the official Python website.

   Verify the installation:
   ```bash
   python3 --version
   pip3 --version
   ```

1. **Install Rust and Cargo:**

   The project requires Rust and its package manager, Cargo. Install them using rustup.
   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```
   
   After installation, adding cargo to the PATH in ~/.bash_profile and then source the file to apply.
   ```bash
   export PATH="$HOME/.cargo/bin:$PATH"
   source ~/.bash_profile
   ```

   Verify the installation by running:
   ```bash
   rustc --version
   cargo --version
   ```

1. **Install Circom:**

   Circom is a circuit compiler for zkSNARKs. Clone the Circom repository and build it:
   ```bash
   git clone https://github.com/iden3/circom.git
   cd circom
   cargo build --release
   cargo install --path circom
   ```

   After installation, verify by running:
   ```bash
   circom --version
   ```

1. **Install SnarkJS:**

   SnarkJS is a JavaScript library for zkSNARKs. Install it globally using npm:
   ```bash
   npm install -g snarkjs
   ```
   Verify the installation:
   ```bash
   snarkjs --version
   ```

1. **Install Additional Node.js Packages:**

   Navigate to the project directory and install the required Node.js packages listed in package.json.
   ```bash
   cd zkp-data-quality
   npm install
   ```
   This command installs the dependencies specified in the package.json file.

By completing these steps, you will have all the necessary software dependencies installed to run the project natively on your system.


### 2. Installation Using Docker

To streamline the setup process, you can utilize Docker to run the project without installing Node.js or npm locally. 
The Docker container has all the software dependencies and mounts the host machine to access the project code. 
See the Dockerfile and entrypoint.sh files for more details.  

1. **Install Docker:**

   If not already installed, you can download it from the [official Docker website](https://www.docker.com/get-started/).

2. **Pull or build the Docker Image:**

   To retrieve the pre-built Docker image from Docker Hub:

   ```bash
   docker pull namenottaken/zkp-environment:latest
   ```

   To build the Dockerfile locally and list it:

   ```bash
   cd zkp-data-quality
   docker build -t namenottaken/zkp-environment:latest .
   docker images
   ```

## Usage

1. **Clone the project repo:**

   For both native and docker usage, clone this project and enter the folder.  
   In addition retrieve the powers-of-tau files used in the zksnark setup ceremony.

   ```bash
   git clone https://github.com/jondowson/zkp-data-quality.git
   cd zkp-data-quality
   mkdir ptau
   wget -P ptau/ https://hermez.s3-eu-west-1.amazonaws.com/powersOfTau28_hez_final_16.ptau
   wget -P ptau/ https://hermez.s3-eu-west-1.amazonaws.com/powersOfTau28_hez_final_22.ptau
   ```

1. **Run the application:**

   Each subfolder of the lib folder creates a proof for a certain dataset attribute, e.g. uniqueness of rows.  
   See the lib folder to see all available proofs.

   To run natively use this command:

   ```bash
   node lib/<lib_folder>/main.js <lib_folder>/your_dataset.csv
   // Example:
   node lib/unique_rows/main.js unique_rows/d100.csv
   ```
   
   To run as a docker container use this command:
   ```bash
   docker run --rm -v "$(pwd)":/app namenottaken/zkp-environment:latest <lib_folder> <data_file>
   // Example - note that main.js is assumed as the first passed parameter.
   docker run --rm -v "$(pwd)":/app namenottaken/zkp-environment:latest unique_rows unique_rows/d100.csv
   ```

## Project Structure

This tree shows the general structure of the project.

```
zkp-data-quality/
data
├── complete
│   ├── c10.csv
│   └── nc10.csv
└── unique_rows
    ├── d10.csv
    ├── d100.csv
    ├── d1000.csv
    ├── u10.csv
    ├── u100.csv
    └── u1000.csv
lib
├── unique_rows
│   ├── circom_templates
│   │   ├── circuit_main.js
│   │   ├── circuit_merkleroot.js
│   │   └── circuit_unique.js
│   ├── config.js
│   ├── functions
│   │   ├── exec_command.js
│   │   ├── merkle_functions.js
│   │   ├── ptau_picker.js
│   │   └── write_dynamic_circuit.js
│   ├── generateZKProof.js
│   ├── main.js
│   └── setup.js
└── utils
    └── generate_csv.js
├── generated/
│   ├── circom/
│   ├── snarkjs/
│   └── scripts/
├── config.js
├── package.json
└── README.md
```

- `data/`: Contains the input datasets in CSV format.
- `lib/unique_rows/`: Houses the main scripts for setup, proof generation, and verification.
- `lib/unique_rows/functions/`: Includes utility functions for Merkle Tree construction and dynamically writing the circom circuits.
- `generated/`: Stores generated files such as Circom circuits, SnarkJS outputs, and scripts. Cleared after every run.
- `config.js`: Configuration file specifying directory paths and other settings.

## ISO Standards  

ISO Standards Focused on Data Quality.

1. **ISO 8000: Data Quality**  
This series of standards provides guidelines and requirements for data quality management, particularly for master data.
Key parts:
ISO 8000-1: Overview of data quality.  
ISO 8000-2: Vocabulary.  
ISO 8000-110: Syntax, semantic encoding, and master data exchange.  
ISO 8000-61X series: Quality measurement methodologies.  

2. **ISO/IEC 25012: Data Quality Model**  
Defines data quality characteristics and associated attributes for data used in computer systems.   
Inherent Data Quality:   
- Accuracy  
- Consistency  
- Credibility  
- Currentness  
- System-Dependent Data Quality:
- Accessibility
- Compliance
- Confidentiality
- Efficiency

3. **ISO/IEC 25024: Measurement of Data Quality**  
Provides a methodology to measure the data quality characteristics defined in ISO/IEC 25012.  

4. **ISO 27001: Information Security Management**  
While primarily about information security, it includes data quality as part of the broader context of managing data integrity and availability.  

5. **ISO 9001: Quality Management Systems**  
This general quality management standard includes principles that can be applied to ensure data quality within the broader quality management system.  

6. **ISO/IEC 38500: IT Governance**  
Addresses data quality in the context of IT governance by emphasizing accountability and proper use of IT systems, which impacts data quality.  

7. **ISO 15489: Records Management**  
Ensures the quality of data and records management, especially in terms of completeness, authenticity, and reliability.  

Additional Standards Related to Specific Contexts: 
- ISO 21597: Information container for linked document delivery: Focuses on data sharing and quality for the built environment.  
- ISO 30300 Series: Ensures metadata and record management systems adhere to quality standards.   

## Contributing

We welcome contributions! Please fork the repository and submit a pull request with your changes. Ensure that your code adheres to the project's coding standards and includes appropriate tests.

## License

This project is licensed under the MIT License. See the LICENSE file for details.

## Acknowledgments

Special thanks to the developers and contributors of the open-source libraries and tools utilized in this project, including circomlibjs and snarkjs.
