# CouchDB for Obsidian LiveSync - Documentation

## Configuration

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `username` | CouchDB admin username | `admin` |
| `password` | CouchDB admin password | (required) |
| `database` | Database name for Obsidian sync | `obsidian-livesync` |

### Example Configuration

```yaml
username: admin
password: your_secure_password_here
database: obsidian-livesync
```

## Network

The add-on exposes CouchDB on port **5984**. This port is accessible from your local network.

- **CouchDB API**: `http://<homeassistant-ip>:5984`
- **Fauxton UI**: `http://<homeassistant-ip>:5984/_utils`

## Setting Up Obsidian LiveSync

### 1. Install the Plugin

1. Open Obsidian Settings
2. Go to **Community Plugins**
3. Search for "Self-hosted LiveSync"
4. Install and enable the plugin

### 2. Configure the Plugin

1. Open plugin settings
2. Set the Remote Database configuration:
   - **URI**: `http://<homeassistant-ip>:5984`
   - **Username**: Your configured username
   - **Password**: Your configured password
   - **Database**: Your configured database name

3. Click **Test Database Connection** to verify

### 3. Initial Sync

- For a new setup: Enable **Live Sync**
- To sync existing vault: Use **Replicate** to push/pull data

## Data Storage

All CouchDB data is stored in `/data/couchdb` within the add-on container, which persists across restarts and updates.

## Troubleshooting

### Cannot Connect

1. Verify the add-on is running
2. Check the add-on logs for errors
3. Ensure port 5984 is not blocked by firewall
4. Verify credentials match between add-on config and Obsidian plugin

### Sync Issues

1. Check Obsidian LiveSync plugin logs
2. Verify the database exists in Fauxton (`http://<ip>:5984/_utils`)
3. Ensure sufficient disk space

### View Logs

Navigate to the add-on page and click **Log** to see CouchDB output.

## Security Notes

- Set a strong password - the database is accessible on your network
- Consider using a reverse proxy with HTTPS for external access
- The database contains your notes - treat credentials securely

## References

- [Obsidian LiveSync Plugin](https://github.com/vrtmrz/obsidian-livesync)
- [CouchDB Documentation](https://docs.couchdb.org/)
- [docker-obsidian-livesync-couchdb](https://github.com/oleduc/docker-obsidian-livesync-couchdb)
