# Cybox-to-Stix-Document-Project
I have a bit of code that I am having trouble finishing up Ive got it printing to XML but I need it to go to a stix document

The program looks a bit like this 

from cybox.objects.image_file_object import ImageFile
from cybox.common import String, Integer, PositiveInteger

#  pip install exifread
import exifread

f = open(Your file path)
exif = exifread.process_file(f)
f.close()

# Convert byte array to unicode
for k in ['Image XPTitle', 'Image XPComment', 'Image XPAuthor', 'Image XPKeywords', 'Image XPSubject']:
    if k in exif:
        exif[k].values = u"".join(map(unichr, exif[k].values)).decode('utf-16')


#################################
# cybox stuff
#################################

# build the cybox record
imgfile = ImageFile()
imgfile.file_name = "sag.jpg"
imgfile.file_path = "Your File Path"

# use extracted data from the image (earlier section)
# http://twigstechtips.blogspot.com/2014/06/python-reading-exif-and-iptc-tags-from.html
# load the cybox record with real data

if 'BitsPerSample' in exif:
    imgfile.bits_per_pixel = exif['Image ImageDescription'].values

if 'ImageWidth' in exif:
    imgfile.image_width = tagdict['ImageWidth']

if 'ImageLength' in exif:
    imgfile.image_width = tagdict['ImageLength']

#########################################
# print out resulting cybox xml record
#########################################
print(imgfile.to_xml(include_namespaces=True, encoding=None))

from pprint import pprint

# python-stix
from stix.core import STIXPackage


def main():
    FILENAME = 'sample.xml'

    # Parse input file
    stix_package = STIXPackage.from_xml(FILENAME)
