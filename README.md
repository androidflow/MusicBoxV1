<h1 align="center"> Virtual Tourist </h1> <br>

<h4 align="center"> View map showing pins, which uses Flickr's API to load images when selecting pin. </h4> <br>
 

## Intro

This project allows you to view pins loaded on a map from CoreData. You'll be able to place pins, and then tap a pin to geographically load Flickr's API data for images at the location. 
<p align="center">
  <img alt="onthemap" title="onthemap" src="screenshots/virtualt1.gif" width=300>
</p>
<br>

## Functions 

* Place pins on map which will save to CoreData.
* API controllers to load Flickr image data geographically. 
* Mapview controller to display pins. 
* Storyboard with combined MapView and CollectionView.
<br>

## Displaying images from CoreData

One of the areas in the app which made alot of use of CoreData was the CollectionView. It was interesting seeing how the CollectionView could simultaneously fill and refresh more than a dozen images concurrently.   

``` swift
 func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell
    {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "PhotoCell", for: indexPath) as! PhotoCell
        cell.activityIndicator.isHidden = true
        cell.imageView.image = nil
        let photo = fetchedResultsController?.object(at: indexPath) as! Photo
        if photo.imageData == nil{
            cell.activityIndicator.isHidden = false
            cell.imageView.image = UIImage(named: "defaultImage")
            cell.activityIndicator.startAnimating()
            DispatchQueue.main.async{
                self.flickr.downloadPhotos(photo.url!){ (image, error) in
                    photo.imageData = image
                    cell.imageView.image = UIImage(data: image as! Data)
                    cell.activityIndicator.isHidden = true
                }
            }
        }else{
            cell.imageView.image = UIImage(data: photo.imageData as! Data)
        }
        return cell
    }
```
<br>

## Article Tips

Some good articles for tips : <br>
* <a href="https://www.yudiz.com/working-with-unwind-segues-in-swift" target="_blank">Working with Segue unwinds in Swift</a><br>
* <a href="https://blog.supereasyapps.com/30-auto-layout-best-practices/#layout-ui-for-one-iphone" target="_blank">30 Auto Layout Best Practices</a>
