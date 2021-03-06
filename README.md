# IIIF Condition Reports

## Project overview

This tool is aimed toward the creation and management of condition reports in cultural heritage institutions, such as museums. It relies on a [Datahub](https://github.com/thedatahub/Datahub) as its source for information about objects for which the reports are to be created.


## Requirements

* php-imagick

The following tables have to be present in MySQL:

```
DROP TABLE IF EXISTS inventory_numbers;
CREATE TABLE `inventory_numbers` (
  `inventory_number` VARCHAR(100) NOT NULL,
  `id` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  PRIMARY KEY (`inventory_number`),
  KEY(`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

DROP TABLE IF EXISTS datahub_data;
CREATE TABLE `datahub_data` (
  `id` INT UNSIGNED NOT NULL,
  `name` VARCHAR(100) NOT NULL,
  `value` TEXT NOT NULL,
  PRIMARY KEY (`id`,`name`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

DROP TABLE IF EXISTS reports;
CREATE TABLE `reports` (
  `id` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  `base_id` INT UNSIGNED DEFAULT NULL,
  `inventory_id` INT UNSIGNED NOT NULL,
  `timestamp` TIMESTAMP NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

DROP TABLE IF EXISTS report_history;
CREATE TABLE `report_history` (
  `id` INT UNSIGNED NOT NULL,
  `previous_id` INT UNSIGNED NOT NULL,
  `sort_order` INT UNSIGNED NOT NULL,
  PRIMARY KEY (`id`, `previous_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

DROP TABLE IF EXISTS report_data;
CREATE TABLE `report_data` (
  `id` INT UNSIGNED NOT NULL,
  `name` VARCHAR(100) NOT NULL,
  `value` TEXT NOT NULL,
  PRIMARY KEY (`id`,`name`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

DROP TABLE IF EXISTS images;
CREATE TABLE `images` (
  `hash` CHAR(64) NOT NULL,
  `image` TEXT NOT NULL,
  `thumbnail` TEXT NOT NULL,
  PRIMARY KEY(`hash`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

DROP TABLE IF EXISTS annotations;
CREATE TABLE `annotations` (
  `image` CHAR(64) NOT NULL,
  `report_id` INT UNSIGNED NOT NULL,
  `annotation_id` VARCHAR(255) NOT NULL,
  `annotation` LONGTEXT NOT NULL,
  PRIMARY KEY (`image`, `report_id`, `annotation_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

DROP TABLE IF EXISTS deleted_annotations;
CREATE TABLE `deleted_annotations` (
  `image` CHAR(64) NOT NULL,
  `report_id` INT UNSIGNED NOT NULL,
  `annotation_id` VARCHAR(255) NOT NULL,
  PRIMARY KEY (`image`, `report_id`, `annotation_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

DROP TABLE IF EXISTS organisations;
CREATE TABLE `organisations` (
  `id` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  `alias` VARCHAR(255) DEFAULT NULL,
  `name` VARCHAR(255) NOT NULL,
  `function` VARCHAR(255) DEFAULT NULL,
  `logo` VARCHAR(255) DEFAULT NULL,
  `vat` VARCHAR(255) DEFAULT NULL,
  `address` VARCHAR(255) DEFAULT NULL,
  `postal` VARCHAR(255) DEFAULT NULL,
  `city` VARCHAR(255) DEFAULT NULL,
  `country` VARCHAR(255) DEFAULT NULL,
  `email` VARCHAR(255) DEFAULT NULL,
  `website` VARCHAR(255) DEFAULT NULL,
  `phone` VARCHAR(255) DEFAULT NULL,
  `mobile` VARCHAR(255) DEFAULT NULL,
  `notes` TEXT DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

DROP TABLE IF EXISTS representatives;
CREATE TABLE `representatives` (
  `id` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  `alias` VARCHAR(255) DEFAULT NULL,
  `name` VARCHAR(255) NOT NULL,
  `function` VARCHAR(255) DEFAULT NULL,
  `email` VARCHAR(255) DEFAULT NULL,
  `phone` VARCHAR(255) DEFAULT NULL,
  `notes` TEXT DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

DROP TABLE IF EXISTS iiif_manifests;
CREATE TABLE IF NOT EXISTS `iiif_manifests` (
  `id` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  `manifest_id` VARCHAR(255) NOT NULL,
  `data` LONGTEXT NOT NULL,
  PRIMARY KEY (`id`)
);
```
