# Get Started

Get started by consuming data from a free, public, Kafka broker.

1. Install the Python library

```sh
pip install quixstreams
```

2. Run this code to consume data from a racing car

```py
from quixstreams import Application

app = Application(broker_address="localhost:9092")

input_topic = app.topic("cardata")
output_topic = app.topic("speed-hopping-windows")

# Converts JSON to a Streaming DataFrame (SDF) tabular format
sdf = app.dataframe(input_topic)

# Calculate hopping window of 1s with 200ms steps
sdf = sdf.apply(lambda row: row["Speed"]) \
      .hopping_window(1000, 200).mean().final() 

# Send rows from SDF back to the output topic as JSON messages
sdf = sdf.to_topic(output_topic)

if __name__ == "__main__":
   app.run(sdf)

```