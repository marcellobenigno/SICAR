# SICAR

This tool is designed for students, researchers, data scientists, or anyone who would like to have access to [SICAR](https://car.gov.br/publico/imoveis/index) files.

## Badges

[![Open In Collab](.github/colab-badge.svg)](https://colab.research.google.com/github/urbanogilson/SICAR/blob/main/examples/colab.ipynb)
[![made-with-python](https://img.shields.io/badge/Made%20with-Python-1f425f.svg)](https://www.python.org/)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![Docker Pulls](https://img.shields.io/docker/pulls/urbanogilson/sicar)](https://hub.docker.com/r/urbanogilson/sicar)
[![Coverage Status](https://coveralls.io/repos/github/urbanogilson/SICAR/badge.svg?branch=main)](https://coveralls.io/github/urbanogilson/SICAR?branch=main)
[![interrogate](.github/interrogate_badge.svg)](https://interrogate.readthedocs.io/)

## Features

- Get cities-codes by state code
- Download Shapefile or CSV
- Download city by code
- Download lists of cities by code
- Download all cities in a state by code
- Download the entire country
- Tesseract, and PaddleOCR (Optional) drivers to automatically detect captcha

## Installation

Install SICAR with pip

```bash
pip install git+https://github.com/urbanogilson/SICAR
```

Prerequisite:

[Google Tesseract OCR](https://github.com/tesseract-ocr/tesseract) (additional info on how to install the engine on Linux, Mac OSX, and Windows).

Optional: [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR) (additional info on how to install the engine on Linux, Mac OSX, and Windows).

If you don't want to install dependencies on your computer or don't know how to install them, we strongly recommend [Google Colab](#run-with-google-colab).

## Documentation

- [SICAR package - API](https://gilsonurbano.com/sicar-api/)
- [SICAR package - What is? Why?](https://gilsonurbano.com/posts/sicar/)

## Usage/Examples

```python
from SICAR import Sicar
import pprint

# Create Sicar instance
car = Sicar(email = "name@domain.com")

# Get cities codes in Roraima state
cities_codes = car.get_cities_codes(state='RR')

pprint.pprint(cities_codes)
# {'Alto Alegre': '1400050',
#  'Amajari': '1400027',
#  'Boa Vista': '1400100',
#  'Bonfim': '1400159',
#  'Cantá': '1400175',
#  'Caracaraí': '1400209',
#  'Caroebe': '1400233',
#  'Iracema': '1400282',
#  'Mucajaí': '1400308',
#  'Normandia': '1400407',
#  'Pacaraima': '1400456',
#  'Rorainópolis': '1400472',
#  'São João da Baliza': '1400506',
#  'São Luiz': '1400605',
#  'Uiramutã': '1400704'}

# Download 'Alto Alegre': '1400050'
car.download_city_code('1400050', folder='Roraima')

# Download in CSV format
from SICAR import OutputFormat
car.download_city_code('1400050', output_format = OutputFormat.CSV, folder='Roraima')

# Download specific cities
cities_codes = {
    'São Gabriel da Cachoeira': '1303809',
    'São Paulo de Olivença': '1303908'
}

car.download_cities(cities_codes=cities_codes, folder='cities')

# Download all cities in Roraima state
car.download_state(state='RR', folder='RR')
```

### OCR drivers

[Optical character recognition (OCR)](https://en.wikipedia.org/wiki/Optical_character_recognition) drivers are used to recognize characters in a captcha.

We currently have two options for automating the download process.

#### [Tesseract OCR](https://github.com/tesseract-ocr/tesseract) (Default)

```python
from SICAR import Sicar
from SICAR.drivers import Tesseract

# Create Sicar instance using Tesseract OCR
car = Sicar(email="name@domain.com", driver=Tesseract)

# Download a city
car.download_cities(cities_codes={'Belo Horizonte': '3106200'}, folder='SICAR/cities')
```

#### [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR)

Install SICAR with pip and include Paddle dependencies

```bash
pip install 'SICAR[paddle] @  git+https://github.com/urbanogilson/SICAR'
```

```python
from SICAR import Sicar
from SICAR.drivers import Paddle

# Create Sicar instance using PaddleOCR
car = Sicar(email="name@domain.com", driver=Paddle)

# Download a city
car.download_cities(cities_codes={'Balneário Camboriú': '4202008'}, folder='SICAR/cities')
```

### Run with Google Colab

Using Google Colab, you don't need to install the dependencies on your computer and you can save files directly to your Google Drive.

[![Open In Collab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/urbanogilson/SICAR/blob/main/examples/colab.ipynb)

### Run with Docker

Pull Image from Docker Hub [urbanogilson/sicar](https://hub.docker.com/r/urbanogilson/sicar)

```sh
docker pull urbanogilson/sicar:latest
```

Run the downloaded Docker Image using an entry point (file) from your machine (host)

```sh
docker run -i -v $(pwd):/sicar urbanogilson/sicar:latest -<./examples/docker.py
```

Note: Update the entry point file [./examples/docker.py](./examples/docker.py) or create a new one to download data based on your needs.

or pass a script through `STDIN`

```sh
docker run -i -v $(pwd):/sicar urbanogilson/sicar:latest -<<EOF
from SICAR import Sicar
from SICAR.drivers import Paddle

car = Sicar(email="name@domain.com", driver=Paddle)

car.download_state(state='MG', folder='MG')
EOF
```

Note: Using `$(pwd)` the container will save the download data into the current folder.

Optional: Make an external directory to store the downloaded data and use a volume parameter in the run command to point to it.

## Acknowledgements

- [Sicar - Sistema Nacional de Cadastro Ambiental Rural](https://www.car.gov.br/)
- [Sicar - Base de Downloads](https://www.car.gov.br/publico/municipios/downloads)

## Roadmap

- [ ] Download city by name
- [x] Make Paddle driver optional
- [x] Add support to download CSV files

## Contributing

The development environment with all necessary packages is available using [Visual Studio Code Dev Containers](https://code.visualstudio.com/docs/remote/containers).

[![Open in Remote - Containers](https://img.shields.io/static/v1?label=Remote%20-%20Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/urbanogilson/SICAR)

Contributions are always welcome!

## Feedback

If you have any feedback, please reach me at hello@gilsonurbano.com

## License

[MIT](LICENSE)
