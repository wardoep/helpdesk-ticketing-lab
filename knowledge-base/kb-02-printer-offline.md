# KB-02 — Fixing a shared printer stuck "Offline"

**Audience:** IT Support agents · **Applies to:** shared/network printers via a print server

## First question: one user or many?

- **Many users, one printer** → the problem is the **server queue or the network/device**, not any single PC. Work the server first.
- **One user, one printer, others fine** → the problem is that **PC** (driver, saved offline state).

## Many-users fix (server side)

1. On the print server, open the queue for the printer. A job at the head in an **Error** state wedges everything behind it.
2. Cancel the stuck job.
3. Restart the spooler: `Restart-Service Spooler` (this clears transient queue corruption).
4. Confirm the printer's **port/IP** still matches the device (a printer that got a new DHCP address will appear offline — give network printers a reservation or static IP).
5. Send a **test page** from the server.

## One-user fix (client side)

1. Uncheck **Use Printer Offline** (right-click the printer).
2. Restart the client spooler.
3. If it persists, remove and re-add the printer / update the driver.

## Prevent repeats

Network printers should have a DHCP reservation or static IP so the queue's port never goes stale. Recurrent jams on one device may mean a driver mismatch — standardise the driver via print-server deployment.
