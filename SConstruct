# -*- python -*-
#
# Setup our environment
#
import glob, os.path
import lsst.SConsUtils as scons

env = scons.makeEnv("imageproc",
                    r"$HeadURL: svn+ssh://svn.lsstcorp.org/DC2/imageproc/tickets/7/SConstruct $",
                    [["boost", "boost/version.hpp", "boost_filesystem:C++"],
                     ["boost", "boost/regex.hpp", "boost_regex:C++"],
                     ["vw", "vw/Core.h", "vw:C++"],
                     ["vw", "vw/Core.h", "vwCore:C++"],
                     ["vw", "vw/Math.h", "vwMath:C++"],
                     ["lapack", None, "lapack", "dgesdd_"],
                     ["mwi", "lsst/mwi/data.h", "mwi:C++"],
                     #["dps", "lsst/dps/Stage.h", "dps:C++"],
                     ["fw", "lsst/fw/MaskedImage.h", "fw"],
                     ["detection", "lsst/detection/Footprint.h", "detection"],
                     ["python", "Python.h"],
                     ["m", "math.h", "m", "sqrt"],
                     ["cfitsio", "fitsio.h", "cfitsio", "ffopen"],
                     ["wcslib", "wcslib/wcs.h", "wcs"],
                     ["xpa", "xpa.h", "xpa", "XPAPuts"],
                     ["minuit", "Minuit/FCNBase.h", "lcg_Minuit:C++"],
                     ])

env.libs["imageproc"] = env.getlibs("boost vw fw cfitsio mwi minuit detection")
env.libs["imageproc"] += ["lapack"]     # bug in scons 1.16; getlibs("lapack") fails as lapack isn't in eups

#
# Build/install things
#
for d in Split("lib src tests examples"):
    SConscript(os.path.join(d, "SConscript"))

# include
for d in Split("include/lsst/%s" % env['eups_product']):
    SConscript(os.path.join(d, "SConscript"))

# python
for d in map(lambda str: os.path.join("python/lsst/%s" % env['eups_product'], str), Split(".")):
    SConscript(os.path.join(d, "SConscript"))

env['IgnoreFiles'] = r"(~$|\.pyc$|^\.svn$|\.o$)"

Alias("install", env.Install(env['prefix'], "python"))
Alias("install", env.Install(env['prefix'], "include"))
Alias("install", env.Install(env['prefix'], "lib"))
Alias("install", env.Install(env['prefix'] + "/bin", glob.glob("bin/*.py")))
Alias("install", env.InstallEups(env['prefix'] + "/ups", glob.glob("ups/*.table")))

scons.CleanTree(r"*~ core *.so *.os *.o")

env.Declare()
env.Help("""
LSST Image Processing Pipeline packages
""")
