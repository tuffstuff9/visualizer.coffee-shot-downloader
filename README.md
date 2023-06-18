# visualizer.coffee Shot Downloader

This is a very simple python script in  a jupyter notebook. It downloads all your shot data from https://visualizer.coffee as JSON files. 

## Limitations

The script only downloads the shot data as a JSON file, as the website does not store the actual .shot files. This script will download ALL your shots, although it can easily be modified to download a specified number. There is no filtering functionality either, although this can also easily be implemented. 

## Usage

You can download the Jupyter notebook file and open it in an environment like Google Colab or run it locally on your system. Make sure to first update the following cell with your visualizer.coffee login credentials:

    email = 'email@example.com' 
    password = 'password'

Once you've updated your credentials, you can simply run the rest of the cells in the notebook in sequence.

The script will fetch all the shot IDs associated with your account, then download the details for each shot. The shot data will be saved in a folder called `shots` in the current directory (the directory where the Jupyter notebook is located).

Each shot's data will be saved as a separate JSON file, with the file named after the shot's ID. For instance, if there is a shot with ID '854d278d-ed35-4d3d-876e-de62dc945db9', the corresponding data will be saved as `shots/854d278d-ed35-4d3d-876e-de62dc945db9.json`.

## Technical Details

visualizer.coffee's public API can be found [here](https://documenter.getpostman.com/view/2402164/UVC2HUik). 

The script uses a combination of standard HTTP requests and asynchronous HTTP requests to gather shot data from visualizer.coffee. Below you will find technical details about the script. 

1. **User Authentication:** These credentials are required for accessing the shot data, as the visualizer.coffee requires user authentication (see public api linked above).

2. **Fetching Shot IDs:** The script uses the `requests` library to send a synchronous GET request to the visualizer.coffee API to retrieve shot IDs. The response from the API includes shot data in batches (100 shots per request). The script retrieves these batches page by page and extracts the shot IDs, which are stored for later use. The process continues until no more shots are available.

3. **Downloading Shot Data:** Instead of making one request at a time, which is time-consuming and inefficient, the script uses the `aiohttp` library to make HTTP requests asynchronously. This means it can send multiple requests at the same time, greatly speeding up the overall download process. For each shot ID, the script constructs an API endpoint URL, sends an asynchronous GET request, and upon receiving a successful response, stores the shot data.

4. **Saving Shot Data:** Once all shot data is received, it's time to store it for offline use. The script saves each shot's data as a separate JSON file in a folder named `shots` in the current directory. 

### API Rate Limits

It is unclear if the visualizer.coffee API has any rate limits in place. However, as a best practice, it's advisable to avoid unnecessary loads on the API servers. 

## Contributing

Feel free to fork the project, open a pull request, or submit issues and feature requests.

## License

[MIT License](https://chat.openai.com/LICENSE)
