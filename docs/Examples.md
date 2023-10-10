# Examples

This section provides examples of Receiver capabilities expressed using the Constraint Sets defined by this specification.

## Audio

Consider an uncompressed audio RTP receiver, that is represented by the following simple [IS-04][] Receiver resource:

```json
{
  "id": "6605bf77-f95b-5d12-bbd7-8c4f98b79b25",
  "version": "1605275841:891227800",
  "label": "",
  "description": "",
  "tags": {
  },
  "device_id": "7652d792-e99f-57ff-a540-faccbf36a3a2",
  "transport": "urn:x-nmos:transport:rtp.mcast",
  "format": "urn:x-nmos:format:audio",
  "interface_bindings": [
    "eth0"
  ],
  "subscription": {
    "active": false,
    "sender_id": null
  },
  "caps": {
    "media_types": [
      "audio/L24",
      "audio/L16"
    ]
  }
}
```

This resource indicates via the `transport` and `format` that it represents an audio RTP receiver.
The `media_types` capability is used to indicate that it supports both 'audio/L24' and 'audio/L16' uncompressed audio.

However, none of these attributes defined in IS-04 are able to express that the receiver can handle a limited number of audio channels, and that the maximum number depends on the packet time.

Using the mechanism defined by this specification and the audio Parameter Constraints listed in the Capabilities register of the [NMOS Parameter Registers][], the receiver's constraints can be expressed by including a `constraint_sets` attribute in the `caps` object, accompanied by a `version` attribute to indicate when the `caps` last changed.

The following example `caps` object indicates the additional constraints of 48 kHz audio and a maximum of 16 channels with 125 microsecond packet time, or 8 channels with 1 millisecond packet time.

```json
  "caps": {
    "media_types": [
      "audio/L24",
      "audio/L16"
    ],
    "constraint_sets": [
      {
        "urn:x-nmos:cap:format:channel_count": {
          "maximum": 16
        },
        "urn:x-nmos:cap:format:sample_rate": {
          "enum": [
            { "numerator": 48000, "denominator": 1 }
          ]
        },
        "urn:x-nmos:cap:transport:packet_time": {
          "enum": [ 0.125 ]
        }
      },
      {
        "urn:x-nmos:cap:format:channel_count": {
          "maximum": 8
        },
        "urn:x-nmos:cap:format:sample_rate": {
          "enum": [
            { "numerator": 48000, "denominator": 1 }
          ]
        },
        "urn:x-nmos:cap:transport:packet_time": {
          "enum": [ 1 ]
        }
      }
    ],
    "version": "1605272395:492335100"
  }
```

For the full example, see:
[examples/receiver-audio.json](../examples/receiver-audio.json)

### ST 2110-30 Conformance Levels

SMPTE ST 2110-30 defines Conformance Levels for audio Receivers.

For an example of how a Receiver can indicate that it supports a specific Conformance Level, see: 
[examples/receiver-audio-level-bx.json](../examples/receiver-audio-level-bx.json)

## Video

The mechanism defined by this specification and the video Parameter Constraints listed in the Capabilities register of the [NMOS Parameter Registers][] can fully represent the video formats and format groups defined by VSF [TR-05][] for interoperability of ST 2110-20.

The following sub-sections provide some examples of how specific formats or format groups can be indicated.

### 720p59.94 Format

In this example, the fixed parameter values for this format are defined.

```json
  "caps": {
    "media_types": [
      "video/raw"
    ],
    "constraint_sets": [
      {
        "urn:x-nmos:cap:meta:label": "720p59.94 Format as per VSF TR-05:2018",
        "urn:x-nmos:cap:format:frame_width": {
          "enum": [ 1280 ]
        },
        "urn:x-nmos:cap:format:frame_height": {
          "enum": [ 720 ]
        },
        "urn:x-nmos:cap:format:interlace_mode": {
          "enum": [ "progressive" ]
        },
        "urn:x-nmos:cap:format:grain_rate": {
          "enum": [
            { "numerator": 60000, "denominator": 1001 }
          ]
        },
        "urn:x-nmos:cap:format:color_sampling": {
          "enum": [ "YCbCr-4:2:2" ]
        },
        "urn:x-nmos:cap:format:component_depth": {
          "enum": [ 10 ]
        },
        "urn:x-nmos:cap:format:transfer_characteristic": {
          "enum": [ "SDR" ]
        },
        "urn:x-nmos:cap:format:colorspace": {
          "enum": [ "BT709" ]
        }
      }
    ],
    "version": "1605272395:492336200"
  }
```

### 1080i Format Group

This format grouping has two members, 1080i50 and 1080i59.94. If a receiver fully implements the group and is currently able to receive either format, a single Constraint Set is able to express this.

```json
  "caps": {
    "media_types": [
      "video/raw"
    ],
    "constraint_sets": [
      {
        "urn:x-nmos:cap:meta:label": "1080i Format Group as per VSF TR-05:2018",
        "urn:x-nmos:cap:format:frame_width": {
          "enum": [ 1920 ]
        },
        "urn:x-nmos:cap:format:frame_height": {
          "enum": [ 1080 ]
        },
        "urn:x-nmos:cap:format:interlace_mode": {
          "enum": [ "interlaced_tff" ]
        },
        "urn:x-nmos:cap:format:grain_rate": {
          "enum": [
            { "numerator": 25, "denominator": 1 },
            { "numerator": 30000, "denominator": 1001 }
          ]
        },
        "urn:x-nmos:cap:format:color_sampling": {
          "enum": [ "YCbCr-4:2:2" ]
        },
        "urn:x-nmos:cap:format:component_depth": {
          "enum": [ 10 ]
        },
        "urn:x-nmos:cap:format:transfer_characteristic": {
          "enum": [ "SDR" ]
        },
        "urn:x-nmos:cap:format:colorspace": {
          "enum": [ "BT709" ]
        }
      }
    ],
    "version": "1605272395:492336200"
  }
```

## High Definition Category

TR-05 defines a high definition category that consists of the three format groups, 720p, 1080i and 1080p.

A receiver could indicate support for the whole category using three Constraint Sets.

```json
    "constraint_sets": [
      {
        "urn:x-nmos:cap:meta:label": "720p Format Group as per VSF TR-05:2018",
        ...
      },
      {
        "urn:x-nmos:cap:meta:label": "1080i Format Group as per VSF TR-05:2018",
        ...
      },
      {
        "urn:x-nmos:cap:meta:label": "1080p Format Group as per VSF TR-05:2018",
        ...
      }
    ]
```

For a full example of a receiver that supports the 1080i and 1080p format groups, see:
[examples/receiver-video-1080.json](../examples/receiver-video-1080.json)

Support for the formats in the ultra high definition category can be indicated similarly.

### Type N, W and A Video Receivers

Receivers are informatively categorized in ST 2110-21 as Type N, Type W and Type A.

For example, Type N receivers can be expected to correctly receive a stream originating from either a Type N or Type NL sender, provided that the receiver is locked to the same clock as the sender. This could be indicated by adding the following Parameter Constraint to each Constraint Set.

```json
        "urn:x-nmos:cap:transport:st2110_21_sender_type": {
          "enum": [
            "2110TPN",
            "2110TPNL"
          ]
        }
```

[IS-04]: https://specs.amwa.tv/is-04/
"AMWA IS-04 NMOS Discovery and Registration Specification"

[NMOS Parameter Registers]: https://specs.amwa.tv/nmos-parameter-registers/
"Common parameter values for AMWA NMOS Specifications"

[TR-05]: https://www.videoservicesforum.org/download/technical_recommendations/VSF_TR-05_2018-06-23.pdf
"VSF TR-05: Essential Formats and Descriptions for Interoperability of SMPTE ST 2110-20 Video Signals"

### Substreams

The following example shows a MUX MPEG 2 transport stream receiver exposing substream constraints.

```json
{
  "id": "9dd002e2-76a0-4edc-88ef-7d4aff4b2d26",
  "transport": "rtp",
  "format": "mux",
  "caps": {
    "media_types": [
      "video/MP2T"
    ],
    "constraint_sets": [
      {
        "urn:x-nmos:substreams": [
          {
            "description": "hi-res",
            "format": "video",
            "count": {
              "enum": [
                1
              ]
            },
            "constraint_sets": [...]
          },
          {
            "description": "proxy",
            "format": "video",
            "count": {
              "minimum": 0,
              "maximum": 1
            },
            "constraint_sets": [...]
          },
          {
            "format": "audio",
            "count": {
              "enum": [
                0,
                2,
                4
              ]
            },
            "constraint_sets": [...]
          }
        ]
      },
      {
        "urn:x-nmos:substreams": [...]
      }
    ]
  }
}
```
