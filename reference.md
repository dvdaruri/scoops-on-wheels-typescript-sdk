# Reference
## Trucks
<details><summary><code>client.trucks.<a href="/src/api/resources/trucks/client/Client.ts">listTrucks</a>() -> ScoopsOnWheels.TruckSummary[]</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Return a summary list of every truck in the fleet.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.trucks.listTrucks();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `TrucksClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.trucks.<a href="/src/api/resources/trucks/client/Client.ts">getTruck</a>({ ...params }) -> ScoopsOnWheels.Truck</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Return full details for a single truck by its ID.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.trucks.getTruck({
    truck_id: "truck_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ScoopsOnWheels.GetTruckTrucksTruckIdGetRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `TrucksClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Search
<details><summary><code>client.search.<a href="/src/api/resources/search/client/Client.ts">searchNearby</a>({ ...params }) -> ScoopsOnWheels.SearchResults</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Search for ice cream trucks near a given coordinate.

Returns trucks sorted by distance, with optional filters for price range,
flavor availability, and open/closed status.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.search.searchNearby({
    latitude: 1.1,
    longitude: 1.1
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ScoopsOnWheels.SearchNearbyTrucksSearchNearbyGetRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `SearchClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Menu
<details><summary><code>client.menu.<a href="/src/api/resources/menu/client/Client.ts">getTruckMenu</a>({ ...params }) -> ScoopsOnWheels.Menu</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieve all menu items for a specific truck.
Optionally filter by maximum price.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.menu.getTruckMenu({
    truck_id: "truck_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ScoopsOnWheels.GetTruckMenuTrucksTruckIdMenuGetRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `MenuClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.menu.<a href="/src/api/resources/menu/client/Client.ts">listMenuItems</a>({ ...params }) -> ScoopsOnWheels.MenuItem[]</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Browse every menu item across all trucks, with optional price and category filters.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.menu.listMenuItems();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ScoopsOnWheels.ListMenuItemsMenuItemsGetRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `MenuClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## System
<details><summary><code>client.system.<a href="/src/api/resources/system/client/Client.ts">health</a>() -> unknown</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Simple liveness probe.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.system.health();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `SystemClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

