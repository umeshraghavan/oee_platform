# OEE Monitoring Platform Solution Architecture (Azure)

## 1. Overview
A cloud-native, multi-tenant platform for real-time monitoring of Overall Equipment Effectiveness (OEE) in manufacturing, using Azure services for scalable, secure, and modular deployment.

## 2. Architecture Diagram (Mermaid)
```mermaid
flowchart TD
    subgraph Edge Devices
        A1[Revolution Pi]
    end
    subgraph IoT & Ingestion
        B1[Sensor Data (MQTT/HTTPS)]
        B2[Azure IoT Hub]
    end
    subgraph Stream Processing
        C1[Azure Stream Analytics]
    end
    subgraph Processed Data Storage
        D1[Azure SQL Database (Processed Sensor Data)]
    end
    subgraph Manual & Reference Data
        E1[Web Application]
        E2[PostgreSQL (Manual Events, Jobs, Users, Thresholds)]
    end
    subgraph Analytics & Integration
        F1[Azure Function (Anomaly Detection)]
    end

    A1 --> B1 --> B2 --> C1 --> D1
    E1 --> E2
    F1 --> E1
    D1 --> F1
    E2 --> F1
```

## 3. Data Flow
- Sensor data flows from Revolution Pi → Azure IoT Hub → Azure Stream Analytics → Azure SQL Database.
- Manual events, jobs, users, and thresholds are managed in the Web Application and stored in PostgreSQL.
- Azure Function fetches data from Azure SQL and PostgreSQL, performs anomaly detection, and returns results to the Web Application.

## 4. Key Azure Services
- **Azure IoT Hub:** Secure ingestion of sensor data.
- **Azure Stream Analytics:** Real-time stream processing.
- **Azure SQL Database:** Stores processed sensor data.
- **Azure Database for PostgreSQL:** Stores manual events, jobs, users, thresholds.
- **Azure Functions:** Serverless compute for analytics/integration.
- **Web Application (React):** Operator/supervisor UI.

## 5. Security & Multi-Tenancy
- All data partitioned by `tenant_id`.
- Authentication via Auth0, Azure AD B2C, or similar.
- Role-based access for operators, supervisors, admins.

## 6. Integration Points
- Sensor data: MQTT/HTTPS → IoT Hub.
- Manual data: Web UI/API → PostgreSQL.
- Analytics: Azure Function joins sensor and manual data for anomaly detection.

## 7. Extensibility
- Easily add new sensor types, machines, or tenants.
- Pluggable analytics via Azure Functions or Stream Analytics queries.

---

*For a visual diagram, paste the Mermaid code above into any Mermaid-compatible editor (e.g., mermaid.live).*
