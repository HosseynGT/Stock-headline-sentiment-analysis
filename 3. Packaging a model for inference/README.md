#  Packaging a model for inference

In the previous step, we exported a TFServe ready model that can easily bind to TFServe container. As I said with a full built-in data transform, there is no worry about inferencing raw text input. I just inject my trained model into TFServe container regarding [this](https://www.tensorflow.org/tfx/serving/docker#creating_your_own_serving_image) TFServe tutorial and It is ready for accepting post requests right after startup.

### Benefits:

- Requests can send over HTTPS for more secure connections
- High throughput for request responding
- The full automation that can help for continuous deploying
- Less dependency means more reliability
- It is not infrastructure dependent, GPU or CPU does not matter you have a working service
- Integration with monitoring tools such as Prometheus

### Drawbacks:

- Static model, you cannot change the model inside the container
- Small memory overhead



## Running Serving container

### Pulling from docker hub or load it locally

```bash
docker pull hossaingt/stock_tfserve:latest
```

And if you want to get the image from a local file:

```bash
docker load < stock_image.tar
```

### Running command

```bash
docker run -t --rm -p 8501:8501 -e MODEL_NAME=stock_sentiment [image name]
```

and that's it!

## Making a sample request

Just a raw string and a URL and you have the results!

```python
import requests

model_server_url = "http://stocksentimentanalysis.azurewebsites.net:8501/v1/models/stock_sentiment:predict"

def predict(text):
    json_data = {
        "signature_name":"serving_default",
        "instances":[text]
    }
    resp = requests.post(model_server_url, json=json_data)
    return resp.json()
```

## Final result

The above response is a float number between 0 and 1. 0 means no like hood for raising of stock and 1 is a highly like hood. With a threshold decision policy it will be:

```
Let predicted value: X

if X=<0.2 then SELL

if 0.2<X<0.8 then HOLD

if X>=0.8 then BUY
```

