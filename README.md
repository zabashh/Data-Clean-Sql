# Nashville Housing dataset Cleaning with SQL

## Standardize Date Format:
In this section, the SaleDate column is converted to a standardized date format.

<pre>
  <code>
    ALTER TABLE SqlCleaning.dbo.NashvilleHousing
    Add SaleDateConverted Date;

    Update SqlCleaning.dbo.NashvilleHousing
    SET SaleDateConverted = CONVERT(Date,SaleDate)

  </code>
</pre>
   ![preview](images/d1.png) | ![preview](images/sd2.png)


## Populate Property Address Data:
In this query we fill in missing PropertyAddress values by using non-null values from other rows with the same ParcelID.
<pre>
  <code>

Select a.ParcelID, a.PropertyAddress, b.ParcelID, b.PropertyAddress, ISNULL(a.PropertyAddress,b.PropertyAddress)
From SqlCleaning.dbo.NashvilleHousing a
JOIN SqlCleaning.dbo.NashvilleHousing b
	on a.ParcelID = b.ParcelID 
	AND a.[UniqueID ] <> b.[UniqueID ]
	where a.PropertyAddress is null

	Update a
	Set PropertyAddress = ISNULL(a.PropertyAddress,b.PropertyAddress)
	From SqlCleaning.dbo.NashvilleHousing a
	JOIN SqlCleaning.dbo.NashvilleHousing b
	on a.ParcelID = b.ParcelID 
	AND a.[UniqueID ] <> b.[UniqueID ]
	where a.PropertyAddress is null

  </code>
</pre>

## Breaking out PropertyAddress into individual columns (Address, City)
New columns, SplitAddress and SplitCity, were added to the table, and existing PropertyAddress values were split into these columns using the PARSENAME and REPLACE functions.

<pre>
	<code>
		Alter Table SqlCleaning.dbo.NashvilleHousing -- Add New Columns 
		ADD 
		SplitAdress	NVARCHAR(255),
		SplitCity NVARCHAR(100);


		UPDATE SqlCleaning.dbo.NashvilleHousing
		 SET SplitAdress = PARSENAME(REPLACE(PropertyAddress,',','.'),2),
			 SplitCity = PARSENAME(REPLACE(PropertyAddress,',','.'),1)
	</code>
</pre>

