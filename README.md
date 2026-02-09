# WildfireSpreadTS - Dataset creation

## Updates 

**Feb 2026:** Thanks to an observant researcher who kindly reached out, an error in the computation of the wind direction is now corrected. I had made an error in using atan(v/u) instead of atan2(v,u) for computing the wind direction. The former loses quadrant information. Thanks for pointing this out!

------

To create the dataset, you will need access to Google Cloud Storage and Google Earth Engine. 

1. Install all necessary requirements from the requirements.txt file via 
```
pip install -r requirements.txt
```
2. Set up the [Google Cloud SDK](https://cloud.google.com/sdk/docs/how-to) and authenticate with your Google account.
3. Set up the [Google Earth Engine Python API](https://developers.google.com/earth-engine/guides/python_install) and authenticate with your Google account.
4. Enter your [Google Service Account](https://cloud.google.com/iam/docs/service-account-overview) credentials and the path to your key file in `main.py`.
5. Set the yaml file in `main.py` that you want to use to download corresponding data and change the Google cloud storage bucket name in the respective yaml file to yours.
6. Run `python main.py` to let GEE compute the dataset and upload it into your Google cloud storage bucket.

The yaml files in `config` contain only the pre-filtered fires that were used in creating the dataset. 


## Downloading the initial list of fires from GlobFire

Above, we use preprocessed yaml files that contain the filtered lists of fires, having removed fire events that 
do not actually contain any fires in the VIIRS active fire product, as well as a low number of fires that had various
data format issues. 

If you do want to recreate the inital, unfiltered list of fires, or vary some parameters in their creation, 
you can paste the content of `GEE_get_GlobFire_data.js` into the GEE interface. This will create a task to download the 
fires for the year that you set in the script. 

Afterwards, you should convert the csv file into a yaml file via

```python3 fire_csv_to_yaml.py --csv_path YOUR_INPUT_CSV_PATH --yaml_path YOUR_OUTPUT_YAML_PATH --year YOUR_YEAR --bucket_name YOUR_GOOGLE_CLOUD_BUCKET```

This yaml file can then be used in step 5 above. Using the provided yamls in `config` would save you some time though.
