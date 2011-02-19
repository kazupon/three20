# variables
IOS_DEVICE_ROOT = /Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS4.2.sdk
IOS_SIMULATOR_ROOT = /Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator4.2.sdk
IOS_DEVICE_SDK = iphoneos4.2
IOS_SIMULATOR_SDK = iphonesimulator4.2
FRAMEWORK_NAME = Three20UINavigator
TARGET_NAME = $(FRAMEWORK_NAME)
TARGET = Release
BUILD_DIR = ../../Build/Products
FRAMEWORK_VERSION_NUMBER = 1.0
FRAMEWORK_BUILD_PATH = $(BUILD_DIR)/$(TARGET)-framework
FRAMEWORK_DIR = $(FRAMEWORK_BUILD_PATH)/$(TARGET_NAME).framework
FRAMEWORK_VERSION = A
PACKAGE_NAME = $(FRAMEWORK_NAME).$(FRAMEWORK_VERSION_NUMBER).tar.gz
DIST_TARGET_DIR = ../../../..
DIST_TARGET_FRAMEWORK = $(DIST_TARGET_DIR)/framework
DIST_TARGET_LIB = $(DIST_TARGET_DIR)/lib

.PHONY: build

all: build pack dist

build:
	@echo "$(FRAMEWORK_NAME) building ..."
	xcodebuild -parallelizeTargets -target $(TARGET_NAME) -configuration $(TARGET) \
		-sdk $(IOS_SIMULATOR_SDK)
	xcodebuild -parallelizeTargets -target $(TARGET_NAME) -configuration $(TARGET) \
		-sdk $(IOS_DEVICE_SDK)
	@echo "... build done !!"

pack:
	@echo "$(FRAMEWORK_NAME) packaging ..."
	rm -rf $(FRAMEWORK_BUILD_PATH)/$(PACKAGE_NAME)
	rm -rf $(FRAMEWORK_BUILD_PATH)/$(FRAMEWORK_NAME).framework 
	mkdir -p $(FRAMEWORK_DIR)
	mkdir -p $(FRAMEWORK_DIR)/Versions
	mkdir -p $(FRAMEWORK_DIR)/Versions/$(FRAMEWORK_VERSION)
	mkdir -p $(FRAMEWORK_DIR)/Versions/$(FRAMEWORK_VERSION)/Resources
	mkdir -p $(FRAMEWORK_DIR)/Versions/$(FRAMEWORK_VERSION)/Headers 
	ln -s $(FRAMEWORK_VERSION) $(FRAMEWORK_DIR)/Versions/Current
	ln -s Versions/Current/Headers $(FRAMEWORK_DIR)/Headers
	ln -s Versions/Current/Resources $(FRAMEWORK_DIR)/Resources
	ln -s Versions/Current/$(FRAMEWORK_NAME) $(FRAMEWORK_DIR)/$(FRAMEWORK_NAME)
	lipo -create $(BUILD_DIR)/$(TARGET)-iphoneos/lib$(FRAMEWORK_NAME).a \
		$(BUILD_DIR)/$(TARGET)-iphonesimulator/lib$(FRAMEWORK_NAME).a \
		-output $(FRAMEWORK_DIR)/Versions/Current/$(FRAMEWORK_NAME)
	cp $(BUILD_DIR)/three20/$(FRAMEWORK_NAME)/*.h $(FRAMEWORK_DIR)/Headers/
	tar -cvzf $(FRAMEWORK_BUILD_PATH)/$(PACKAGE_NAME) $(FRAMEWORK_DIR)
	@echo "... packaging done !!"

dist:
	@echo "$(FRAMEWORK_NAME) deployment ..."
	rm -rf $(DIST_TARGET_FRAMEWORK)/$(TARGET)/$(TARGET_NAME).framework
	rm -rf ../../lib/$(TARGET)-ios/lib$(FRAMEWORK_NAME).a
	rm -rf ../../lib/$(TARGET)-ios-simulator/lib$(FRAMEWORK_NAME).a
	rm -rf ../../lib/$(TARGET)-universal/lib$(FRAMEWORK_NAME).a
	cp -R $(FRAMEWORK_DIR) $(DIST_TARGET_FRAMEWORK)/$(TARGET)/
	cp $(BUILD_DIR)/$(TARGET)-iphoneos/lib$(FRAMEWORK_NAME).a $(DIST_TARGET_LIB)/$(TARGET)-ios/lib$(FRAMEWORK_NAME).a
	cp $(BUILD_DIR)/$(TARGET)-iphonesimulator/lib$(FRAMEWORK_NAME).a $(DIST_TARGET_LIB)/$(TARGET)-ios-simulator/lib$(FRAMEWORK_NAME).a
	cp $(FRAMEWORK_DIR)/Versions/Current/$(FRAMEWORK_NAME) $(DIST_TARGET_LIB)/$(TARGET)-universal/lib$(FRAMEWORK_NAME).a
	@echo "... deployment done !!"

clean:
	@echo "$(FRAMEWORK_NAME) claening ..."
	xcodebuild -parallelizeTargets clean
	rm $(BUILD_DIR)/lib$(FRAMEWORK_NAME).a
	@echo "... clean done !!"

rebuild: clean build

