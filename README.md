# Halo Sensor API Documentation

**Base URL:** `http://localhost:8000/api/`

---

## Table of Contents
1. [Users Management](#users-management)
2. [User Groups](#user-groups)
3. [Sensors](#sensors)
4. [Sensor Groups](#sensor-groups)
5. [Areas](#areas)
6. [Alerts](#alerts)
7. [Sensor Readings & Data](#sensor-readings--data)

---

## Users Management

### Create User
**POST** `/api/users/create/`

**Request Body:**
```json
{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "securepassword123",
  "first_name": "John",
  "last_name": "Doe",
  "role": "admin"
}
```

**Response (201 Created):**
```json
{
  "id": 1,
  "username": "john_doe",
  "email": "john@example.com",
  "first_name": "John",
  "last_name": "Doe",
  "role": "admin",
  "message": "User 'john_doe' created successfully"
}
```

---

### List All Users
**GET** `/api/users/`

**Query Parameters:**
- `search` - Search by username, email, first_name, last_name, or role
- `ordering` - Order by `user__username`, `user__date_joined`, or `created_at`

**Response (200 OK):**
```json
[
  {
    "id": 1,
    "username": "john_doe",
    "email": "john@example.com",
    "first_name": "John",
    "last_name": "Doe",
    "role": "admin",
    "is_active": true,
    "groups": [
      {
        "id": 1,
        "name": "Security",
        "description": "Security team members"
      }
    ],
    "created_at": "2026-01-15T10:00:00Z",
    "updated_at": "2026-01-15T10:00:00Z"
  }
]
```

---

### Get User Details
**GET** `/api/users/{id}/`

**Response (200 OK):**
```json
{
  "id": 1,
  "user": {
    "id": 1,
    "username": "john_doe",
    "email": "john@example.com",
    "first_name": "John",
    "last_name": "Doe"
  },
  "role": "admin",
  "is_active": true,
  "groups": [
    {
      "id": 1,
      "name": "Security",
      "description": "Security team members"
    }
  ],
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T10:00:00Z"
}
```

---

### Update User
**PUT/PATCH** `/api/users/{id}/`

**Request Body (PATCH):**
```json
{
  "role": "viewer",
  "is_active": true,
  "email": "newemail@example.com",
  "first_name": "Jane",
  "last_name": "Smith"
}
```

**Response (200 OK):**
```json
{
  "id": 1,
  "user": {
    "id": 1,
    "username": "john_doe",
    "email": "newemail@example.com",
    "first_name": "Jane",
    "last_name": "Smith"
  },
  "role": "viewer",
  "is_active": true,
  "groups": [],
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T12:30:00Z"
}
```

---

### Delete User
**DELETE** `/api/users/{id}/`

**Response (204 No Content)**

---

### Get Current User Profile
**GET** `/api/users/profile/me/`

**Response (200 OK):**
```json
{
  "id": 1,
  "username": "john_doe",
  "email": "john@example.com",
  "first_name": "John",
  "last_name": "Doe",
  "role": "admin",
  "is_active": true,
  "groups": [
    {
      "id": 1,
      "name": "Security",
      "description": "Security team members"
    }
  ]
}
```

---

### Change Password
**POST** `/api/users/change-password/`

**Request Body:**
```json
{
  "old_password": "currentpassword123",
  "new_password": "newpassword456"
}
```

**Response (200 OK):**
```json
{
  "message": "Password changed successfully"
}
```

---

## User Groups

### Create User Group
**POST** `/api/user-groups/`

**Request Body:**
```json
{
  "name": "Security Team",
  "description": "Security personnel who monitor alerts",
  "member_ids": [1, 2, 3]
}
```

**Response (201 Created):**
```json
{
  "id": 1,
  "name": "Security Team",
  "description": "Security personnel who monitor alerts",
  "members": [
    {
      "id": 1,
      "username": "john_doe",
      "email": "john@example.com",
      "first_name": "John",
      "last_name": "Doe",
      "role": "admin",
      "is_active": true,
      "groups": [],
      "created_at": "2026-01-15T10:00:00Z",
      "updated_at": "2026-01-15T10:00:00Z"
    }
  ],
  "member_count": 1,
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T10:00:00Z"
}
```

---

### List User Groups
**GET** `/api/user-groups/`

**Query Parameters:**
- `search` - Search by name or description
- `ordering` - Order by `name` or `created_at`

**Response (200 OK):**
```json
[
  {
    "id": 1,
    "name": "Security Team",
    "description": "Security personnel",
    "members": [...],
    "member_count": 3,
    "created_at": "2026-01-15T10:00:00Z",
    "updated_at": "2026-01-15T10:00:00Z"
  }
]
```

---

### Get User Group Details
**GET** `/api/user-groups/{id}/`

**Response (200 OK):**
```json
{
  "id": 1,
  "name": "Security Team",
  "description": "Security personnel",
  "members": [
    {
      "id": 1,
      "username": "john_doe",
      "email": "john@example.com",
      "first_name": "John",
      "last_name": "Doe",
      "role": "admin",
      "is_active": true,
      "groups": [],
      "created_at": "2026-01-15T10:00:00Z",
      "updated_at": "2026-01-15T10:00:00Z"
    },
    {
      "id": 2,
      "username": "jane_smith",
      "email": "jane@example.com",
      "first_name": "Jane",
      "last_name": "Smith",
      "role": "viewer",
      "is_active": true,
      "groups": [],
      "created_at": "2026-01-15T10:01:00Z",
      "updated_at": "2026-01-15T10:01:00Z"
    }
  ],
  "member_count": 2,
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T10:00:00Z"
}
```

---

### Update User Group
**PUT/PATCH** `/api/user-groups/{id}/`

**Request Body:**
```json
{
  "name": "Security Team Updated",
  "description": "Updated security personnel group",
  "member_ids": [1, 2, 3, 4]
}
```

**Response (200 OK):**
```json
{
  "id": 1,
  "name": "Security Team Updated",
  "description": "Updated security personnel group",
  "members": [...],
  "member_count": 4,
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T11:30:00Z"
}
```

---

### Add Members to Group
**POST** `/api/user-groups/{id}/add_members/`

**Request Body:**
```json
{
  "member_ids": [4, 5, 6]
}
```

**Response (200 OK):**
```json
{
  "id": 1,
  "name": "Security Team",
  "description": "Security personnel",
  "members": [...],
  "member_count": 6,
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T11:30:00Z"
}
```

---

### Remove Members from Group
**POST** `/api/user-groups/{id}/remove_members/`

**Request Body:**
```json
{
  "member_ids": [4, 5]
}
```

**Response (200 OK):**
```json
{
  "id": 1,
  "name": "Security Team",
  "description": "Security personnel",
  "members": [...],
  "member_count": 4,
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T11:30:00Z"
}
```

---

### Delete User Group
**DELETE** `/api/user-groups/{id}/`

**Response (204 No Content)**

---

## Sensors

### Create Sensor
**POST** `/api/sensors/`

**Request Body:**
```json
{
  "name": "Hallway-01",
  "sensor_type": "HALO_3C",
  "location": "Building A, Floor 2, Hallway",
  "is_active": true,
  "ip_address": "192.168.1.100",
  "mac_address": "00:1A:2B:3C:4D:5E",
  "area": 1,
  "x_min": 0.0,
  "y_min": 0.0,
  "x_max": 10.0,
  "y_max": 10.0,
  "z_min": 0.0,
  "z_max": 3.0,
  "radius": 5.0
}
```

**Response (201 Created):**
```json
{
  "id": 1,
  "name": "Hallway-01",
  "sensor_type": "HALO_3C",
  "location": "Building A, Floor 2, Hallway",
  "is_active": true,
  "is_online": false,
  "ip_address": "192.168.1.100",
  "mac_address": "00:1A:2B:3C:4D:5E",
  "area": 1,
  "sensor_groups": [],
  "x_min": 0.0,
  "y_min": 0.0,
  "x_max": 10.0,
  "y_max": 10.0,
  "z_min": 0.0,
  "z_max": 3.0,
  "radius": 5.0,
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T10:00:00Z"
}
```

---

### List Sensors
**GET** `/api/sensors/`

**Query Parameters:**
- `search` - Search by name, sensor_type, location, mac_address, ip_address
- `is_active` - Filter by active status (true/false)
- `sensor_type` - Filter by sensor type (HALO_3C, HALO_IOT, etc.)
- `is_online` - Filter by online status
- `ordering` - Order by created_at, name, is_active, sensor_type

**Response (200 OK):**
```json
[
  {
    "id": 1,
    "name": "Hallway-01",
    "sensor_type": "HALO_3C",
    "location": "Building A, Floor 2",
    "is_active": true,
    "is_online": true,
    "ip_address": "192.168.1.100",
    "mac_address": "00:1A:2B:3C:4D:5E",
    "area": 1,
    "created_at": "2026-01-15T10:00:00Z",
    "updated_at": "2026-01-15T10:00:00Z"
  }
]
```

---

### Get Sensor Details
**GET** `/api/sensors/{id}/`

**Response (200 OK):**
```json
{
  "id": 1,
  "name": "Hallway-01",
  "sensor_type": "HALO_3C",
  "location": "Building A, Floor 2, Hallway",
  "is_active": true,
  "is_online": true,
  "ip_address": "192.168.1.100",
  "mac_address": "00:1A:2B:3C:4D:5E",
  "area": 1,
  "sensor_groups": [
    {
      "id": 1,
      "name": "Hallways",
      "description": "All hallway sensors",
      "sensors": [...],
      "sensor_count": 5,
      "created_at": "2026-01-15T10:00:00Z",
      "updated_at": "2026-01-15T10:00:00Z"
    }
  ],
  "sensor_configurations": [
    {
      "id": 1,
      "sensor_name": "temp_c",
      "enabled": true,
      "min_value": 15.0,
      "threshold": 25.0,
      "max_value": 35.0,
      "created_at": "2026-01-15T10:00:00Z",
      "updated_at": "2026-01-15T10:00:00Z"
    }
  ],
  "x_min": 0.0,
  "y_min": 0.0,
  "x_max": 10.0,
  "y_max": 10.0,
  "z_min": 0.0,
  "z_max": 3.0,
  "radius": 5.0,
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T10:00:00Z"
}
```

---

### Update Sensor
**PUT/PATCH** `/api/sensors/{id}/`

**Request Body:**
```json
{
  "name": "Hallway-01-Updated",
  "location": "Building A, Floor 2, West Hallway",
  "is_active": true
}
```

**Response (200 OK):**
```json
{
  "id": 1,
  "name": "Hallway-01-Updated",
  "sensor_type": "HALO_3C",
  "location": "Building A, Floor 2, West Hallway",
  "is_active": true,
  "is_online": true,
  "ip_address": "192.168.1.100",
  "mac_address": "00:1A:2B:3C:4D:5E",
  "area": 1,
  "sensor_groups": [],
  "sensor_configurations": [],
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T11:30:00Z"
}
```

---

### Delete Sensor
**DELETE** `/api/sensors/{id}/`

**Response (204 No Content)**

---

## Sensor Groups

### Create Sensor Group
**POST** `/api/sensor-groups/`

**Request Body:**
```json
{
  "name": "Hallways",
  "description": "All hallway sensors across the building"
}
```

**Response (201 Created):**
```json
{
  "id": 1,
  "name": "Hallways",
  "description": "All hallway sensors across the building",
  "sensors": [],
  "sensor_count": 0,
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T10:00:00Z"
}
```

---

### List Sensor Groups
**GET** `/api/sensor-groups/`

**Query Parameters:**
- `search` - Search by name or description
- `ordering` - Order by name or created_at

**Response (200 OK):**
```json
[
  {
    "id": 1,
    "name": "Hallways",
    "description": "All hallway sensors",
    "sensors": [
      {
        "id": 1,
        "name": "Hallway-01",
        "sensor_type": "HALO_3C",
        "is_active": true,
        "is_online": true
      },
      {
        "id": 2,
        "name": "Hallway-02",
        "sensor_type": "HALO_3C",
        "is_active": true,
        "is_online": false
      }
    ],
    "sensor_count": 2,
    "created_at": "2026-01-15T10:00:00Z",
    "updated_at": "2026-01-15T10:00:00Z"
  }
]
```

---

### Get Sensor Group Details
**GET** `/api/sensor-groups/{id}/`

**Response (200 OK):**
```json
{
  "id": 1,
  "name": "Hallways",
  "description": "All hallway sensors",
  "sensors": [
    {
      "id": 1,
      "name": "Hallway-01",
      "sensor_type": "HALO_3C",
      "is_active": true,
      "is_online": true
    },
    {
      "id": 2,
      "name": "Hallway-02",
      "sensor_type": "HALO_3C",
      "is_active": true,
      "is_online": false
    }
  ],
  "sensor_count": 2,
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T10:00:00Z"
}
```

---

### Update Sensor Group
**PUT/PATCH** `/api/sensor-groups/{id}/`

**Request Body:**
```json
{
  "name": "Hallways Updated",
  "description": "All hallway sensors - updated"
}
```

**Response (200 OK):**
```json
{
  "id": 1,
  "name": "Hallways Updated",
  "description": "All hallway sensors - updated",
  "sensors": [...],
  "sensor_count": 2,
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T11:30:00Z"
}
```

---

### Delete Sensor Group
**DELETE** `/api/sensor-groups/{id}/`

**Response (204 No Content)**

---

## Areas

### Create Area
**POST** `/api/areas/`

**Request Body:**
```json
{
  "name": "Building A",
  "area_type": "building",
  "parent": null,
  "offset_x": 0.0,
  "offset_y": 0.0,
  "offset_z": 0.0,
  "scale_factor": 1.0
}
```

**Response (201 Created):**
```json
{
  "id": 1,
  "name": "Building A",
  "area_type": "building",
  "parent": null,
  "sensor_count": 0,
  "offset_x": 0.0,
  "offset_y": 0.0,
  "offset_z": 0.0,
  "scale_factor": 1.0,
  "area_plan": null,
  "person_in_charge": [],
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T10:00:00Z"
}
```

---

### List Areas
**GET** `/api/areas/`

**Query Parameters:**
- `search` - Search by name
- `ordering` - Order by name or created_at

**Response (200 OK):**
```json
[
  {
    "id": 1,
    "name": "Building A",
    "area_type": "building",
    "parent": null,
    "sensor_count": 5,
    "offset_x": 0.0,
    "offset_y": 0.0,
    "offset_z": 0.0,
    "scale_factor": 1.0,
    "created_at": "2026-01-15T10:00:00Z",
    "updated_at": "2026-01-15T10:00:00Z"
  }
]
```

---

### Get Area Details
**GET** `/api/areas/{id}/`

**Response (200 OK):**
```json
{
  "id": 1,
  "name": "Building A",
  "area_type": "building",
  "parent": null,
  "subareas": [
    {
      "id": 2,
      "name": "Floor 1",
      "area_type": "floor",
      "parent": 1
    }
  ],
  "sensors": [
    {
      "id": 1,
      "name": "Hallway-01",
      "sensor_type": "HALO_3C",
      "is_active": true
    }
  ],
  "sensor_count": 5,
  "offset_x": 0.0,
  "offset_y": 0.0,
  "offset_z": 0.0,
  "scale_factor": 1.0,
  "person_in_charge": [
    {
      "id": 1,
      "username": "manager_user",
      "email": "manager@example.com"
    }
  ],
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T10:00:00Z"
}
```

---

### Update Area
**PUT/PATCH** `/api/areas/{id}/`

**Request Body:**
```json
{
  "name": "Building A - Main",
  "area_type": "building",
  "scale_factor": 1.5,
  "person_in_charge": [1, 2, 3]
}
```

**Response (200 OK):**
```json
{
  "id": 1,
  "name": "Building A - Main",
  "area_type": "building",
  "parent": null,
  "sensor_count": 5,
  "offset_x": 0.0,
  "offset_y": 0.0,
  "offset_z": 0.0,
  "scale_factor": 1.5,
  "area_plan": null,
  "person_in_charge": [
    {"id": 1, "username": "user1", "email": "user1@example.com"},
    {"id": 2, "username": "user2", "email": "user2@example.com"},
    {"id": 3, "username": "user3", "email": "user3@example.com"}
  ],
  "created_at": "2026-01-15T10:00:00Z",
  "updated_at": "2026-01-15T11:30:00Z"
}
```

---

### Delete Area
**DELETE** `/api/areas/{id}/`

**Response (204 No Content)**

---

## Alerts

### Create Alert
**POST** `/api/alerts/`

**Request Body:**
```json
{
  "sensor": 1,
  "alert_type": "Temperature Threshold Exceeded",
  "alert_value": "28.5°C",
  "assigned_user_ids": [1, 2],
  "assigned_group_ids": [1]
}
```

**Response (201 Created):**
```json
{
  "id": 1,
  "sensor_id": 1,
  "sensor_name": "Hallway-01",
  "alert_type": "Temperature Threshold Exceeded",
  "alert_value": "28.5°C",
  "assigned_to_users": [
    {
      "id": 1,
      "username": "john_doe",
      "email": "john@example.com",
      "first_name": "John",
      "last_name": "Doe",
      "role": "admin",
      "is_active": true,
      "groups": [],
      "created_at": "2026-01-15T10:00:00Z",
      "updated_at": "2026-01-15T10:00:00Z"
    }
  ],
  "assigned_to_groups": [
    {
      "id": 1,
      "name": "Security Team",
      "description": "Security personnel",
      "members": [...],
      "member_count": 3,
      "created_at": "2026-01-15T10:00:00Z",
      "updated_at": "2026-01-15T10:00:00Z"
    }
  ],
  "is_resolved": false,
  "is_read": false,
  "created_at": "2026-01-15T10:00:00Z",
  "resolved_at": null,
  "read_at": null
}
```

---

### List Alerts
**GET** `/api/alerts/`

**Query Parameters:**
- `search` - Search by sensor name or alert type
- `sensor_id` - Filter by sensor
- `is_resolved` - Filter by resolution status
- `is_read` - Filter by read status
- `ordering` - Order by created_at or sensor

**Response (200 OK):**
```json
[
  {
    "id": 1,
    "sensor_id": 1,
    "sensor_name": "Hallway-01",
    "alert_type": "Temperature Threshold Exceeded",
    "alert_value": "28.5°C",
    "assigned_to_users": [...],
    "assigned_to_groups": [...],
    "is_resolved": false,
    "is_read": false,
    "created_at": "2026-01-15T10:00:00Z",
    "resolved_at": null,
    "read_at": null
  }
]
```

---

### Get Alert Details
**GET** `/api/alerts/{id}/`

**Response (200 OK):**
```json
{
  "id": 1,
  "sensor_id": 1,
  "sensor_name": "Hallway-01",
  "alert_type": "Temperature Threshold Exceeded",
  "alert_value": "28.5°C",
  "assigned_to_users": [...],
  "assigned_to_groups": [...],
  "is_resolved": false,
  "is_read": false,
  "created_at": "2026-01-15T10:00:00Z",
  "resolved_at": null,
  "read_at": null
}
```

---

### Update Alert
**PUT/PATCH** `/api/alerts/{id}/`

**Request Body:**
```json
{
  "is_read": true,
  "is_resolved": true,
  "assigned_user_ids": [1, 2, 3],
  "assigned_group_ids": [1, 2]
}
```

**Response (200 OK):**
```json
{
  "id": 1,
  "sensor_id": 1,
  "sensor_name": "Hallway-01",
  "alert_type": "Temperature Threshold Exceeded",
  "alert_value": "28.5°C",
  "assigned_to_users": [...],
  "assigned_to_groups": [...],
  "is_resolved": true,
  "is_read": true,
  "created_at": "2026-01-15T10:00:00Z",
  "resolved_at": "2026-01-15T11:30:00Z",
  "read_at": "2026-01-15T11:30:00Z"
}
```

---

### Delete Alert
**DELETE** `/api/alerts/{id}/`

**Response (204 No Content)**

---

## Sensor Readings & Data

### Get Latest Readings
**GET** `/api/readings/latest/`

**Response (200 OK):**
```json
[
  {
    "id": 1,
    "device_name": "Hallway-01",
    "ip_address": "192.168.1.100",
    "mac_address": "00:1A:2B:3C:4D:5E",
    "timestamp": "2026-01-15T12:00:00Z",
    "event_id": "EVT_001",
    "event_source": "motion_detection",
    "event_value": 1.0,
    "event_threshold": 0.5,
    "active_events": "motion,temp",
    "sensor_data": {
      "temperature": 22.5,
      "humidity": 45.3,
      "co2": 450,
      "light_level": 500
    },
    "firmware_version": "1.0.1",
    "building_wing": "A",
    "building_floor": "2",
    "building_room": "Hallway",
    "created_at": "2026-01-15T12:00:00Z"
  }
]
```

---

### Get All Sensors Latest Data
**GET** `/api/sensors/latest-data/`

**Response (200 OK):**
```json
[
  {
    "sensor_id": 1,
    "sensor_name": "Hallway-01",
    "sensor_type": "HALO_3C",
    "is_online": true,
    "latest_reading": {
      "timestamp": "2026-01-15T12:00:00Z",
      "sensor_data": {
        "temperature": 22.5,
        "humidity": 45.3,
        "co2": 450
      },
      "event_id": "EVT_001",
      "active_events": "motion"
    }
  }
]
```

---

### Get Device Readings
**GET** `/api/devices/{device_id}/readings`

**Query Parameters:**
- `start_time` - Start timestamp (ISO format)
- `end_time` - End timestamp (ISO format)
- `limit` - Number of readings to return

**Response (200 OK):**
```json
[
  {
    "id": 1,
    "device_name": "Hallway-01",
    "timestamp": "2026-01-15T12:00:00Z",
    "sensor_data": {...},
    "event_id": "EVT_001",
    "active_events": "motion,temp"
  }
]
```

---

### Get Device Latest Data
**GET** `/api/devices/{device_id}/latest`

**Response (200 OK):**
```json
{
  "device_name": "Hallway-01",
  "mac_address": "00:1A:2B:3C:4D:5E",
  "ip_address": "192.168.1.100",
  "timestamp": "2026-01-15T12:00:00Z",
  "sensor_data": {
    "temperature": 22.5,
    "humidity": 45.3,
    "co2": 450,
    "light_level": 500
  },
  "active_events": "motion",
  "firmware_version": "1.0.1"
}
```

---

### Get Active Events
**GET** `/api/events/active/`

**Response (200 OK):**
```json
{
  "total_active_events": 5,
  "events": [
    {
      "sensor_id": 1,
      "sensor_name": "Hallway-01",
      "event_type": "motion",
      "event_value": 1.0,
      "timestamp": "2026-01-15T12:00:00Z"
    },
    {
      "sensor_id": 2,
      "sensor_name": "Hallway-02",
      "event_type": "temperature",
      "event_value": 28.5,
      "timestamp": "2026-01-15T11:55:00Z"
    }
  ]
}
```

---

### Get Heartbeat Status
**GET** `/api/heartbeat/status/`

**Response (200 OK):**
```json
[
  {
    "sensor_id": 1,
    "sensor_name": "Hallway-01",
    "is_online": true,
    "last_heartbeat": "2026-01-15T12:00:00Z",
    "heartbeat_interval": 60,
    "heartbeat_status": "healthy"
  },
  {
    "sensor_id": 2,
    "sensor_name": "Hallway-02",
    "is_online": false,
    "last_heartbeat": "2026-01-15T10:30:00Z",
    "heartbeat_interval": 60,
    "heartbeat_status": "offline"
  }
]
```

---

### Halo Data Ingestion
**POST** `/api/halo/data`

**Request Body:**
```json
{
  "device_name": "Hallway-01",
  "mac_address": "00:1A:2B:3C:4D:5E",
  "ip_address": "192.168.1.100",
  "timestamp": "2026-01-15T12:00:00Z",
  "sensor_data": {
    "temperature": 22.5,
    "humidity": 45.3,
    "co2": 450,
    "pm2_5": 12.5,
    "light_level": 500,
    "noise": 45.5
  },
  "active_events": "motion,aggression",
  "firmware_version": "1.0.1",
  "building_wing": "A",
  "building_floor": "2",
  "building_room": "Hallway"
}
```

**Response (201 Created):**
```json
{
  "message": "Data ingested successfully",
  "sensor_id": 1,
  "reading_id": 1,
  "timestamp": "2026-01-15T12:00:00Z"
}
```

---

### Heartbeat Event
**POST** `/api/halo/heartbeat`

**Request Body:**
```json
{
  "device_name": "Hallway-01",
  "mac_address": "00:1A:2B:3C:4D:5E",
  "ip_address": "192.168.1.100",
  "timestamp": "2026-01-15T12:00:00Z",
  "firmware_version": "1.0.1",
  "active_events": "motion",
  "sensor_readings": {
    "temperature": 22.5,
    "humidity": 45.3,
    "co2": 450
  }
}
```

**Response (201 Created):**
```json
{
  "message": "Heartbeat recorded",
  "sensor_id": 1,
  "device_name": "Hallway-01",
  "timestamp": "2026-01-15T12:00:00Z"
}
```

---

## Error Responses

### 400 Bad Request
```json
{
  "error": "Invalid request data",
  "details": {
    "field_name": ["Error message"]
  }
}
```

### 401 Unauthorized
```json
{
  "detail": "Authentication credentials were not provided."
}
```

### 403 Forbidden
```json
{
  "detail": "You do not have permission to perform this action."
}
```

### 404 Not Found
```json
{
  "detail": "Not found."
}
```

### 500 Internal Server Error
```json
{
  "error": "Internal server error",
  "message": "Error details"
}
```

---

## Authentication

All endpoints (except `/api/halo/data` and `/api/halo/heartbeat`) require authentication.

**Headers:**
```
Authorization: Token YOUR_AUTH_TOKEN
```

Or use Django's session authentication for development.

---

## Pagination

List endpoints support pagination:

**Query Parameters:**
- `page` - Page number (default: 1)
- `page_size` - Results per page (default: 20)

**Response includes:**
```json
{
  "count": 50,
  "next": "http://localhost:8000/api/sensors/?page=2",
  "previous": null,
  "results": [...]
}
```

---

**Last Updated:** January 15, 2026
