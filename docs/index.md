# Supplant API Documentation

## Overview

This document provides the necessary information for external companies to integrate with Supplant's API, specifically for retrieving detailed data on growers and plots related to a specified root reseller, including irrigation recommendations and satellite data.

---

## Authentication

All API calls require authentication via an API Key provided by Supplant back office upon agreement signing.

**Headers:**

```
x-api-key: <provided_api_key>
```

---

## Endpoint Details

### `POST /api/v1/growerPlotsData`

**Description**  
Retrieves comprehensive plot data, including irrigation recommendations and satellite data for a specified root reseller and all related growers and plots in its hierarchy.

#### Request

- **Method:** POST  
- **URL:** `/api/v1/growerPlotsData`  
- **Headers:**
  ```
  Content-Type: application/json
  x-api-key: <provided_api_key>
  ```
- **Body:**
  ```json
  {
    "rootResellerId": "<UUID>"
  }
  ```

#### Successful Response

- **Status:** 200 OK  
- **Body:**
  ```json
  [
    {
      "plotId": "string",
      "regionalManagerName": "string",
      "growerName": "string",
      "plotName": "string",
      "plotTz": "string",
      "growerId": "string",
      "plotSize": float,
      "currentDataDate": "YYYY-MM-DD",
      "oneMonthBackDate": "YYYY-MM-DD",
      "irrigationPlan": {
        "pulses": { "timestamp": float },
        "pulsesInTime": { "timestamp": "string" }
      },
      "satelliteData": {
        "vegetationCoverScore": float,
        "vegetationUniformityScore": float,
        "soilMoistureRootZone": float,
        "vegetationCoverChange": float,
        "vegetationUniformityChange": float,
        "soilMoistureRootZoneChange": float,
        "vegetationCoverLink": "URL",
        "vegetationUniformityLink": "URL",
        "soilMoistureRootZoneLink": "URL"
      }
    }
  ]
  ```

---

## Example Response

```json
[
  {
    "plotId": "550e8400-e29b-41d4-a716-446655440000",
    "regionalManagerName": "John Doe",
    "growerName": "Green Growers",
    "plotName": "Plot 21",
    "plotTz": "Europe/Lisbon",
    "growerId": "550e8400-e29b-41d4-a716-446655441111",
    "plotSize": 12.5,
    "currentDataDate": "2025-03-31",
    "oneMonthBackDate": "2025-02-28",
    "irrigationPlan": {
      "pulses": {
        "2025-03-31T00:00:00Z": 5.5,
        "2025-04-01T00:00:00Z": 4.8
      },
      "pulsesInTime": {
        "2025-03-31T00:00:00Z": "2.5",
        "2025-04-01T00:00:00Z": "2.2"
      }
    },
    "satelliteData": {
      "vegetationCoverScore": 78.6,
      "vegetationUniformityScore": 89.3,
      "soilMoistureRootZone": 34.7,
      "vegetationCoverChange": -2.4,
      "vegetationUniformityChange": 1.8,
      "soilMoistureRootZoneChange": -0.6,
      "vegetationCoverLink": "https://satellite.example.com/plots/550e8400-e29b-41d4-a716-446655440000/vegetation_cover.png",
      "vegetationUniformityLink": "https://satellite.example.com/plots/550e8400-e29b-41d4-a716-446655440000/vegetation_uniformity.png",
      "soilMoistureRootZoneLink": "https://satellite.example.com/plots/550e8400-e29b-41d4-a716-446655440000/soil_moisture.png"
    }
  }
]
```

---

## Error Responses

| Status Code | Message              | Description                                  |
|-------------|----------------------|----------------------------------------------|
| 400         | Bad Request           | Invalid request body or missing parameters   |
| 401         | Unauthorized          | Missing or invalid API key                   |
| 403         | Access Denied         | API key lacks permissions                    |
| 404         | Not Found             | No data found for provided reseller ID       |
| 429         | Too Many Requests     | Exceeded daily limit (10 requests/day)       |
| 500         | Internal Server Error | Server-side error occurred                   |

---

## Rate Limits

- **Limit:** 10 requests per day per API key  
- **Response on breach:** 429 Too Many Requests

---

## Support

For further assistance, contact **SupPlant support** at:  
📧 `support@supplant.me`
