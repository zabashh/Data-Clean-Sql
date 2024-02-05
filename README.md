# Nashville Housing dataset Cleaning with SQL

## Standardize Date Format:
In this section, the SaleDate column is converted to a standardized date format.

<pre>
  <code>
    ALTER TABLE SqlCleaning.dbo.NashvilleHousing
    Add SaleDateConverted Date;

    Update SqlCleaning.dbo.NashvilleHousing
    SET SaleDateConverted = CONVERT(Date,SaleDate)

   | ![preview](images/d1.png) | ![preview](images/sd2.png) 
    

    

  </code>
</pre>
