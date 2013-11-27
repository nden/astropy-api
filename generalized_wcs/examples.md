These are examples of how an instrument scientist can use the package to construct a WCS for an instrument. All examples assume the WCS information was already read in from somewhere (either a FITS header or a JSON file). This does not deal with WCS serialization iCS:
)
    class ImagingWCS(WCS):
         """
         An example [dist_info](https://github.com/nden/code-experiments/blob/master/generalized_wcs_api/prototype/jwst_example/reference_files/distortion_image.json) JSON file.
         wcs_info is a dict of basic FITS WCS keywords from the FITS header
         
         """
        def __init__(wcs_info, distortion_info):
              dist = self.create_distortion_transformstion(distortion_info)
              linear_trans = self.create_linear_wcs_transformation(wcs_info)
              coord_sys = self.create_coordinate_system(wcs_info)
              foc2sky = self.create_foc2sky(wcs_info)
              trans = modeling.SerialCompositeModel([dist, linear_trans, foc2sky_trans])
              super(ImagingWCS, self).__init__(trans, coord_sys)

        def create_linear_wcs_transformation(self, wcs_info):
             """Creates a composite transformation of a shift, rotation and scaling"""
             return linear_trans

        def create_distortion_transformation(self, dist_info):
            """
            Creates a distortion transformation.
            
            An example of a JSON representation.
            """
            return dist_trans

            
        def create_foc2sky(self, wcs_info):
            """ Create a transformation from focal plane to sky - deprojection + celestial rotation"""
            return foc2sky

       def create_coordinate_system(self, wcs_info):
           """Construts an instance of wsc.CoordinateSystem"""
           return coord_sys            


Spectral WCS:

    class SpectralWCS(WCS):

        """ Example [spec_info](https://github.com/nden/code-experiments/blob/master/generalized_wcs_api/prototype/jwst_example/reference_files/spec_wcs.json) """

        def __init__(self, spec_info):
            trans = self.create_spectral_transform(spec_info)
            coord_sys = self.create_coordinate_system(wcs_info)
            super(SpectralWCS, self).__init__(trans, coord_sys)

         def create_coordinate_system(self, wcs_info):
           """Construts an instance of wsc.CoordinateSystem"""
           return coord_sys
            
         def create_spectral_transform(spec_info):
             """ Constructs a transform from teh model(s) in sepc_info"""
             return spec_model

An examplel of a composite spectral transform

    class CompositeSpectralTransform(object):
    
    def __init__(self, wcs_info, spec_info, dist_info=None):
        self.spatial_transform = ImagingTransform(wcs_info, dist_info)
        self.spectral_transform = SpectralTransform(spec_info)

    def __call__(self, x, y):
        alpha, delta = self.spatial_transform(x, y)
        wave = self.spectral_transform(x)
        return alpha, delta, wave


IFU Example:

    class IFUWCS(WCS):
          """ 
          Examples of JSON reference files.
          
          [regions_def](https://github.com/nden/code-experiments/blob/master/generalized_wcs_api/prototype/jwst_example/reference_files/regions_miri.json)
          [wcs_regions](https://github.com/nden/code-experiments/blob/master/generalized_wcs_api/prototype/jwst_example/reference_files/wcs_regions.json)
          [spec_regions](https://github.com/nden/code-experiments/blob/master/generalized_wcs_api/prototype/jwst_example/reference_files/spec_regions.json)
          
          """
        def __init__(self, regions_def, wcs_info, wcs_regions, spec_regions, dist_info):
             trans = RegionsSelector(mask_shape, regions_def, wcs_info, wcs_regions, spec_regions, dist_info )
             coord_sys = self.create_coordinate_system(wcs_info)
             super(IFUWCS, self).__init__(trans, coord_sys)

         def create_coordinate_system(self, wcs_info):
           """Construts an instance of wsc.CoordinateSystem"""
           return coord_sys
