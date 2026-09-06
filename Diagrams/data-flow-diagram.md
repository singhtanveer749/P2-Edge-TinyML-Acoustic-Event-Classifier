# Detailed System Block and Data-Flow Diagram
## P2 – Edge TinyML Acoustic Event Classifier

```mermaid
flowchart TD
    A[Physical Sound<br/>Background / Clap / Whistle]
    --> B[INMP441 MEMS Microphone]
    B -->|I2S Digital Audio| C[ESP32 DevKit v1]
    C --> D[16 kHz Audio Sampling]
    D --> E[1 Second Audio Window]
    E --> F[MFE Feature Extraction]
    F --> G[Edge Impulse TinyML Model<br/>Quantized int8]
    G --> H{Classification Result}
    H --> I[Background]
    H --> J[Clap]
    H --> K[Whistle]
    I --> L[Ignore Background<br/>No Cloud Event]
    J --> M{Confidence >= 0.80?}
    K --> M
    M -->|No| N[Low Confidence<br/>Event Ignored]
    M -->|Yes| O{5 Second Cooldown Finished?}
    O -->|No| P[Duplicate Event Ignored]
    O -->|Yes| Q[Event Accepted]
    Q --> R[Create Event Telemetry]
    R --> S[Event Code<br/>1 = Clap<br/>2 = Whistle]
    R --> T[Confidence Score]
    R --> U[Model Version<br/>v1.0.0 / Code 100]
    R --> V[Inference Time<br/>Approx. 74-86 ms]
    S --> W[Wi-Fi]
    T --> W
    U --> W
    V --> W
    W -->|HTTPS REST POST| X[ThingSpeak Private Channel]
    X --> Y[Field 1: Event Code]
    X --> Z[Field 2: Confidence]
    X --> AA[Field 3: Model Version]
    X --> AB[Field 4: Inference Time]
    AB --> AC[Cloud Dashboard<br/>Monitoring and Historical Analysis]
    C -. Raw audio remains local .-> AD[No Raw Audio Uploaded]
```

## Data-Flow Explanation

The INMP441 microphone captures physical sound and sends digital audio to the ESP32 DevKit v1 through the I2S interface.

The ESP32 samples the audio at 16 kHz and processes approximately one-second audio windows locally. MFE feature extraction converts the audio into features suitable for the Edge Impulse TinyML classifier.

The quantized int8 model classifies each audio window as background, clap, or whistle.

Background classifications are ignored and are not sent to the cloud.

For clap and whistle, the ESP32 checks whether the confidence is at least 0.80. Low-confidence classifications are ignored.

A five-second cooldown is used after an accepted event to reduce repeated event uploads.

For an accepted event, only small telemetry values are transmitted:
- Event code
- Confidence score
- Model version
- Inference time

The ESP32 sends this telemetry over Wi-Fi using an HTTPS REST request to a private ThingSpeak channel.

Raw audio remains on the ESP32 and is not uploaded to ThingSpeak. This reduces bandwidth use and improves privacy.

## Actual Prototype Results
- Model Testing Accuracy: 76.67%
- Background live confidence: up to 0.996
- Clap live confidence: up to 0.957
- Whistle live confidence: up to 0.996
- Event confidence threshold: 0.80
- Cooldown: 5 seconds
- Live processing time: approximately 74-86 ms
- Cloud platform: ThingSpeak
- Cloud protocol: HTTPS / REST
