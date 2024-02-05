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
