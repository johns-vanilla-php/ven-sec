-- phpMyAdmin SQL Dump
-- version 5.2.1
-- https://www.phpmyadmin.net/
--
-- Host: localhost:3306
-- Generation Time: Mar 03, 2025 at 06:48 PM
-- Server version: 10.6.21-MariaDB-cll-lve
-- PHP Version: 8.3.15

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
START TRANSACTION;
SET time_zone = "-05:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;

--
-- Database: `vs_db`
--

-- --------------------------------------------------------

--
-- Table structure for table `attendance`
--

CREATE TABLE `attendance` (
  `attendance_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `worksite_id` int(11) NOT NULL,
  `scheduled_start` datetime NOT NULL,
  `scheduled_end` datetime NOT NULL,
  `check_in_time` datetime DEFAULT NULL,
  `check_out_time` datetime DEFAULT NULL,
  `late_arrival` enum('Yes','No') DEFAULT 'No',
  `early_departure` enum('Yes','No') DEFAULT 'No',
  `missed_in_out` enum('Yes','No') DEFAULT 'No',
  `status` enum('present','absent','late','early','missed') DEFAULT 'present',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `audit_logs`
--

CREATE TABLE `audit_logs` (
  `log_id` int(11) NOT NULL,
  `action_type` enum('user_creation','role_change','payroll_update','contract_modification','shift_assignment','incident_report') NOT NULL,
  `action_details` text NOT NULL,
  `user_id` int(11) NOT NULL,
  `affected_id` int(11) DEFAULT NULL,
  `affected_table` enum('users','contracts','payroll','shift_assignments','incident_reports') NOT NULL,
  `action_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `created_at` timestamp NOT NULL DEFAULT current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `clients`
--

CREATE TABLE `clients` (
  `client_id` int(11) NOT NULL,
  `client_number` varchar(8) DEFAULT NULL,
  `client_name` varchar(255) DEFAULT NULL,
  `admin_id` int(11) NOT NULL DEFAULT 0,
  `client_type` varchar(255) DEFAULT NULL,
  `industry` varchar(50) DEFAULT NULL,
  `notes` text DEFAULT NULL,
  `contact_name` varchar(100) DEFAULT NULL,
  `contact_email` varchar(100) DEFAULT NULL,
  `contact_phone` varchar(20) DEFAULT NULL,
  `address_line1` varchar(255) DEFAULT NULL,
  `address_line2` varchar(255) DEFAULT NULL,
  `city` varchar(100) DEFAULT NULL,
  `state` varchar(50) DEFAULT NULL,
  `zip_code` varchar(20) DEFAULT NULL,
  `country` varchar(50) DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `companies`
--

CREATE TABLE `companies` (
  `company_id` int(11) NOT NULL,
  `company_name` varchar(255) NOT NULL,
  `company_type` enum('security','client','vendor') NOT NULL,
  `contact_name` varchar(100) DEFAULT NULL,
  `contact_email` varchar(100) DEFAULT NULL,
  `contact_phone` varchar(20) DEFAULT NULL,
  `registration_number` varchar(100) DEFAULT NULL,
  `payment_terms` enum('net-30','prepaid','custom') DEFAULT 'net-30',
  `custom_terms` varchar(255) DEFAULT NULL,
  `address_line1` varchar(255) DEFAULT NULL,
  `address_line2` varchar(255) DEFAULT NULL,
  `city` varchar(100) DEFAULT NULL,
  `state` varchar(50) DEFAULT NULL,
  `zip_code` varchar(20) DEFAULT NULL,
  `country` varchar(50) DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `contracts`
--

CREATE TABLE `contracts` (
  `contract_id` int(11) NOT NULL,
  `company_id` int(11) NOT NULL,
  `client_id` int(11) NOT NULL,
  `worksite_id` int(11) NOT NULL,
  `contract_type` enum('annual','temporary','ad-hoc') NOT NULL,
  `start_date` date NOT NULL,
  `end_date` date DEFAULT NULL,
  `contract_terms` text DEFAULT NULL,
  `status` enum('active','inactive','pending','expired','negotiating') DEFAULT 'pending',
  `total_contract_value` float(11,2) DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `contract_amendments`
--

CREATE TABLE `contract_amendments` (
  `amendment_id` int(11) NOT NULL,
  `contract_id` int(11) NOT NULL,
  `amendment_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `amendment_details` text DEFAULT NULL,
  `amended_by` int(11) NOT NULL,
  `amendment_status` enum('pending','approved','denied','clarify') DEFAULT 'pending',
  `reviewed_by` int(11) NOT NULL DEFAULT 0,
  `reviewed_date` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00',
  `review_notes` text DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `current_payrates`
--

CREATE TABLE `current_payrates` (
  `rate_id` int(11) NOT NULL,
  `payscale_id` int(11) NOT NULL DEFAULT 0,
  `rate_name` varchar(255) NOT NULL,
  `rate_description` text DEFAULT NULL,
  `rate_type` enum('Normal','Overtime','Holiday','Holiday Overtime','Other') NOT NULL,
  `rate_other_desc` text DEFAULT NULL,
  `status` enum('Pending Approval','Pending Start Date','Denied','Active','Expired') DEFAULT 'Pending Approval',
  `effective_start_date` datetime DEFAULT NULL,
  `effective_end_date` datetime DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `daily_payroll`
--

CREATE TABLE `daily_payroll` (
  `payroll_id` int(11) NOT NULL,
  `user_id` int(11) NOT NULL,
  `worksite_id` int(11) NOT NULL,
  `payscale_id` int(11) NOT NULL,
  `start_date` date NOT NULL,
  `end_date` date NOT NULL,
  `regular_hours` decimal(10,2) NOT NULL DEFAULT 0.00,
  `overtime_hours` decimal(10,2) NOT NULL DEFAULT 0.00,
  `pay_rate` decimal(10,2) NOT NULL DEFAULT 0.00,
  `overtime_rate` decimal(10,2) NOT NULL DEFAULT 0.00,
  `total_pay` decimal(10,2) NOT NULL DEFAULT 0.00,
  `status` enum('approved','flagged','needs_reviewed','denied','pending_paydate','paid') DEFAULT 'pending_paydate',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `emergency_contacts`
--

CREATE TABLE `emergency_contacts` (
  `contact_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `contact_priority` int(11) NOT NULL DEFAULT 1,
  `contact_name` varchar(255) NOT NULL,
  `contact_relationship` varchar(100) NOT NULL,
  `contact_phone` varchar(20) NOT NULL,
  `contact_email` varchar(100) DEFAULT NULL,
  `contact_address` varchar(255) DEFAULT NULL,
  `status` enum('Active','Inactive') NOT NULL DEFAULT 'Active',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `employee_performance_reviews`
--

CREATE TABLE `employee_performance_reviews` (
  `review_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `reviewer_id` int(11) NOT NULL,
  `review_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `performance_score` decimal(5,2) DEFAULT NULL,
  `feedback` text DEFAULT NULL,
  `strengths` text DEFAULT NULL,
  `areas_for_improvement` text DEFAULT NULL,
  `status` enum('pending','approved','denied') DEFAULT 'pending',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `employment_history`
--

CREATE TABLE `employment_history` (
  `history_id` int(11) NOT NULL,
  `user_id` int(11) NOT NULL,
  `company_id` int(11) NOT NULL,
  `client_id` int(11) NOT NULL,
  `worksite_id` int(11) NOT NULL,
  `role_id` int(11) NOT NULL,
  `start_date` date NOT NULL,
  `start_pay_scale` int(11) NOT NULL,
  `end_date` date DEFAULT NULL,
  `end_pay_scale` int(11) DEFAULT NULL,
  `termination_reason` text DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `fueling_stations`
--

CREATE TABLE `fueling_stations` (
  `station_id` int(11) NOT NULL,
  `station_name` varchar(255) NOT NULL,
  `station_address` varchar(255) DEFAULT NULL,
  `fuel_types` enum('Gasoline','Diesel','Electric','Compressed Natural Gas','Other') NOT NULL,
  `contact_phone` varchar(20) DEFAULT NULL,
  `contact_email` varchar(100) DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `incident_reports`
--

CREATE TABLE `incident_reports` (
  `incident_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `worksite_id` int(11) NOT NULL,
  `shift_id` int(11) NOT NULL,
  `incident_date` datetime NOT NULL,
  `incident_type` enum('Accident','Altercation','Theft','Damage','Other') NOT NULL,
  `incident_description` text NOT NULL,
  `actions_taken` text DEFAULT NULL,
  `status` enum('reported','under investigation','resolved') DEFAULT 'reported',
  `report_by` int(11) NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `incident_report_actions`
--

CREATE TABLE `incident_report_actions` (
  `action_id` int(11) NOT NULL,
  `incident_id` int(11) NOT NULL,
  `action_type` enum('Investigation','Corrective Action','Report Filing','Escalation','Other') NOT NULL,
  `action_details` text NOT NULL,
  `action_taken_by` int(11) NOT NULL,
  `action_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `incident_responses`
--

CREATE TABLE `incident_responses` (
  `response_id` int(11) NOT NULL,
  `breach_id` int(11) NOT NULL,
  `response_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `response_type` enum('Immediate Action','Investigation','Follow-up','Preventive Measures') NOT NULL,
  `response_details` text NOT NULL,
  `response_team` text DEFAULT NULL,
  `outcome` enum('Resolved','Ongoing','Unresolved') DEFAULT 'Ongoing',
  `created_by` int(11) NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `leave_history`
--

CREATE TABLE `leave_history` (
  `history_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `leave_type` enum('PTO','Sick Leave','Vacation','Other') NOT NULL,
  `start_date` date NOT NULL,
  `end_date` date NOT NULL,
  `total_hours` float(11,2) NOT NULL DEFAULT 0.00,
  `status` enum('approved','pending','denied') DEFAULT 'pending',
  `supervisor_id` int(11) DEFAULT NULL,
  `approval_date` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `reason` text DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `log_entries`
--

CREATE TABLE `log_entries` (
  `entry_id` int(11) NOT NULL,
  `log_id` int(11) NOT NULL,
  `entry_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `entry_details` text NOT NULL,
  `has_media` enum('Yes','No') DEFAULT 'No',
  `entry_category` int(11) NOT NULL DEFAULT 0,
  `needs_client_attention` enum('Yes','No') DEFAULT 'No',
  `needs_supervisor_attention` enum('Yes','No') DEFAULT 'No',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `log_entry_categories`
--

CREATE TABLE `log_entry_categories` (
  `category_id` int(11) NOT NULL,
  `category_name` varchar(255) NOT NULL,
  `description` text DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `log_entry_media`
--

CREATE TABLE `log_entry_media` (
  `media_id` int(11) NOT NULL,
  `log_entry_id` int(11) NOT NULL,
  `media_type` enum('photo','video','audio','other') NOT NULL,
  `media_url` text NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `notifications`
--

CREATE TABLE `notifications` (
  `notification_id` int(11) NOT NULL,
  `reply_to_notification_id` int(11) DEFAULT 0,
  `user_id` int(11) NOT NULL,
  `notification_type_id` int(11) NOT NULL,
  `message` text NOT NULL,
  `status` enum('unread','read','archived') DEFAULT 'unread',
  `priority` enum('Critical','High','Standard','Low','Routine') DEFAULT NULL,
  `company_id` int(11) DEFAULT NULL,
  `region_id` int(11) NOT NULL,
  `client_id` int(11) DEFAULT NULL,
  `worksite_id` int(11) DEFAULT NULL,
  `regional_supervisor_id` int(11) DEFAULT NULL,
  `site_supervisor_id` int(11) DEFAULT NULL,
  `shift_supervisor_id` int(11) DEFAULT NULL,
  `from_id` int(11) DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `read_at` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00',
  `archived_at` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00'
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `notification_types`
--

CREATE TABLE `notification_types` (
  `notification_type_id` int(11) NOT NULL,
  `type_name` varchar(255) NOT NULL,
  `default_priority` varchar(25) DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `overtime_requests`
--

CREATE TABLE `overtime_requests` (
  `request_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `supervisor_id` int(11) NOT NULL,
  `shift_id` int(11) NOT NULL,
  `requested_hours` decimal(10,2) NOT NULL,
  `reason` text DEFAULT NULL,
  `status` enum('pending','approved','denied') DEFAULT 'pending',
  `approved_by` int(11) DEFAULT NULL,
  `approval_date` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `approval_notes` text DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `payscales`
--

CREATE TABLE `payscales` (
  `payscale_id` int(11) NOT NULL,
  `payscale_name` varchar(255) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `roles`
--

CREATE TABLE `roles` (
  `role_id` int(11) NOT NULL,
  `role_name` varchar(100) NOT NULL,
  `role_description` text DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `scheduled_time_off`
--

CREATE TABLE `scheduled_time_off` (
  `time_off_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `time_off_type` enum('PTO','Sick Leave','Vacation','Other') NOT NULL,
  `start_date` date NOT NULL,
  `end_date` date NOT NULL,
  `total_hours` float(11,2) NOT NULL DEFAULT 0.00,
  `status` enum('approved','pending','denied') DEFAULT 'pending',
  `supervisor_id` int(11) DEFAULT NULL,
  `approval_date` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `reason` text DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `security_breaches`
--

CREATE TABLE `security_breaches` (
  `breach_id` int(11) NOT NULL,
  `worksite_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `breach_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `breach_type` enum('Unauthorized Access','Theft','Vandalism','Security Equipment Failure','Other') NOT NULL,
  `breach_description` text NOT NULL,
  `actions_taken` text DEFAULT NULL,
  `reported_by` int(11) NOT NULL,
  `resolved_by` int(11) DEFAULT NULL,
  `resolution_date` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00',
  `status` enum('reported','under investigation','resolved') DEFAULT 'reported',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `shift_assignments`
--

CREATE TABLE `shift_assignments` (
  `assignment_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `shift_id` int(11) NOT NULL,
  `worksite_id` int(11) NOT NULL,
  `scheduled_start` datetime NOT NULL,
  `scheduled_end` datetime NOT NULL,
  `actual_start` datetime DEFAULT NULL,
  `actual_end` datetime DEFAULT NULL,
  `shift_supervisor` int(11) DEFAULT 0,
  `shift_type` enum('1st Shift','2nd Shift','3rd Shift','Unusual Shift') DEFAULT NULL,
  `status` enum('assigned','completed','pending','cancelled') DEFAULT 'assigned',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `shift_changes`
--

CREATE TABLE `shift_changes` (
  `change_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `original_shift_id` int(11) NOT NULL,
  `new_shift_id` int(11) NOT NULL,
  `change_reason` text NOT NULL,
  `status` enum('pending','approved','denied') DEFAULT 'pending',
  `approved_by` int(11) DEFAULT NULL,
  `approval_date` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `shift_logs`
--

CREATE TABLE `shift_logs` (
  `log_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `shift_id` int(11) NOT NULL,
  `hours_worked` float(11,2) NOT NULL DEFAULT 0.00,
  `overtime_hours` float(11,2) NOT NULL DEFAULT 0.00,
  `status` enum('submitted','approved','rejected') DEFAULT 'submitted',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `shift_reports`
--

CREATE TABLE `shift_reports` (
  `report_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `worksite_id` int(11) NOT NULL,
  `shift_id` int(11) NOT NULL,
  `regional_supervisor_id` int(11) NOT NULL,
  `site_supervisor_id` int(11) DEFAULT 0,
  `shift_supervisor_id` int(11) DEFAULT 0,
  `liaison_id` int(11) NOT NULL,
  `shift_start_date` date NOT NULL,
  `scheduled_start_time` varchar(4) DEFAULT NULL,
  `actual_start_time` datetime NOT NULL,
  `shift_end_date` date NOT NULL,
  `scheduled_end_time` varchar(4) DEFAULT NULL,
  `actual_end_time` datetime DEFAULT NULL,
  `scheduled_hours` float(4,2) DEFAULT NULL,
  `actual_hours` float(4,2) DEFAULT NULL,
  `status` enum('submitted','reviewed','approved','rejected','pending','in progress') DEFAULT 'submitted',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `shift_swaps`
--

CREATE TABLE `shift_swaps` (
  `swap_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `original_shift_id` int(11) NOT NULL,
  `swap_with_officer_id` int(11) NOT NULL,
  `swap_reason` text DEFAULT NULL,
  `status` enum('pending','approved','denied') DEFAULT 'pending',
  `approved_by` int(11) DEFAULT NULL,
  `approval_date` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `system_settings`
--

CREATE TABLE `system_settings` (
  `setting_id` int(11) NOT NULL,
  `setting_name` varchar(255) NOT NULL,
  `setting_value` text NOT NULL,
  `setting_type` enum('integer','float','string','boolean','text') NOT NULL,
  `description` text DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `task_assignments`
--

CREATE TABLE `task_assignments` (
  `task_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `assigned_by` int(11) NOT NULL,
  `task_description` text NOT NULL,
  `status` enum('pending','in_progress','completed','cancelled') DEFAULT 'pending',
  `priority` enum('low','medium','high') DEFAULT 'medium',
  `due_date` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `completed_at` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `tax_rates`
--

CREATE TABLE `tax_rates` (
  `tax_rate_id` int(11) NOT NULL,
  `tax_name` varchar(255) NOT NULL,
  `tax_type` enum('Federal','State','Local','Other') NOT NULL,
  `rate_percentage` decimal(5,2) NOT NULL,
  `description` text DEFAULT NULL,
  `effective_start_date` datetime NOT NULL,
  `effective_end_date` datetime DEFAULT NULL,
  `status` enum('Active','Expired','Pending') DEFAULT 'Active',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `time_off_requests`
--

CREATE TABLE `time_off_requests` (
  `request_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `request_type` enum('PTO','Sick Leave','Vacation','Other') NOT NULL,
  `start_date` date NOT NULL,
  `end_date` date NOT NULL,
  `total_hours` float(11,2) NOT NULL DEFAULT 0.00,
  `reason` text DEFAULT NULL,
  `status` enum('pending','approved','denied') DEFAULT 'pending',
  `approved_by` int(11) DEFAULT NULL,
  `approval_date` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `approval_reason` varchar(255) DEFAULT NULL,
  `covered_by` int(11) DEFAULT 0,
  `caused_ot` enum('Yes','No') DEFAULT 'Yes',
  `projected_earned_hours` float(11,2) NOT NULL DEFAULT 0.00,
  `hours_after_time_off` float(11,2) NOT NULL DEFAULT 0.00,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `training_sessions`
--

CREATE TABLE `training_sessions` (
  `training_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `training_name` varchar(255) NOT NULL,
  `training_date` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `training_score` decimal(5,2) DEFAULT NULL,
  `session_duration` int(11) NOT NULL,
  `completed` enum('Yes','No') DEFAULT 'No',
  `completed_date` datetime DEFAULT NULL,
  `trainer_id` int(11) DEFAULT NULL,
  `is_required` enum('Yes','No') NOT NULL DEFAULT 'Yes',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `users`
--

CREATE TABLE `users` (
  `user_id` int(11) NOT NULL,
  `site_employee` enum('yes','no') DEFAULT 'no',
  `username` varchar(50) NOT NULL,
  `email` varchar(100) NOT NULL,
  `password_hash` varchar(255) NOT NULL,
  `password_updated` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `user_role` int(11) DEFAULT 0,
  `current_company` int(11) DEFAULT 0,
  `current_client` int(11) DEFAULT 0,
  `current_worksite` int(11) DEFAULT 0,
  `status` enum('active','inactive') DEFAULT 'active',
  `theme` int(11) DEFAULT 1,
  `phone_number` varchar(20) DEFAULT NULL,
  `address_line1` varchar(255) DEFAULT NULL,
  `address_line2` varchar(255) DEFAULT NULL,
  `city` varchar(100) DEFAULT NULL,
  `state` varchar(50) DEFAULT NULL,
  `zip_code` varchar(20) DEFAULT NULL,
  `country` varchar(50) DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `last_login` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `failed_logins` int(11) DEFAULT 0,
  `locked` int(11) DEFAULT 0,
  `locked_on` datetime DEFAULT NULL,
  `locked_until` datetime DEFAULT NULL,
  `two_factor_enabled` tinyint(1) DEFAULT 0,
  `two_factor_secret` varchar(255) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_accident_history`
--

CREATE TABLE `vehicle_accident_history` (
  `accident_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `accident_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `accident_type` enum('Collision','Roll-over','Fire','Vandalism','Other') NOT NULL,
  `accident_description` text NOT NULL,
  `involved_party` text DEFAULT NULL,
  `damages_reported` text DEFAULT NULL,
  `insurance_claim_filed` enum('Yes','No') DEFAULT 'No',
  `resolution_status` enum('Under Investigation','Resolved','Closed','Pending') DEFAULT 'Under Investigation',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_assignments`
--

CREATE TABLE `vehicle_assignments` (
  `assignment_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `assignment_start` datetime NOT NULL,
  `assignment_end` datetime NOT NULL,
  `status` enum('assigned','in_use','returned','maintenance','decommissioned') DEFAULT 'assigned',
  `vehicle_condition` text DEFAULT NULL,
  `notes` text DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_damage_claims`
--

CREATE TABLE `vehicle_damage_claims` (
  `damage_claim_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `claim_number` varchar(255) NOT NULL,
  `claim_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `damage_description` text NOT NULL,
  `claim_amount` decimal(10,2) NOT NULL,
  `claim_status` enum('Pending','Approved','Denied','Settled') DEFAULT 'Pending',
  `incident_report_id` int(11) DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_damage_history`
--

CREATE TABLE `vehicle_damage_history` (
  `damage_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `damage_type` enum('Minor','Major','Accident','Vandalism','Other') NOT NULL,
  `damage_description` text NOT NULL,
  `repair_cost` decimal(10,2) DEFAULT NULL,
  `damage_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_fleet_management`
--

CREATE TABLE `vehicle_fleet_management` (
  `fleet_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `vehicle_status` enum('In Service','Out of Service','Under Maintenance','Retired') NOT NULL,
  `registration_status` enum('Active','Expired') DEFAULT 'Active',
  `service_due_date` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `assigned_to` int(11) DEFAULT NULL,
  `worksite_id` int(11) DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_fuel_purchase`
--

CREATE TABLE `vehicle_fuel_purchase` (
  `fuel_entry_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `fuel_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `fuel_type` enum('Gasoline','Diesel','Electric','Other') NOT NULL,
  `fuel_octane` enum('87','88','89','91','93','Other') NOT NULL DEFAULT '89',
  `fuel_quantity` decimal(10,2) NOT NULL,
  `fuel_weight` enum('Gallons','Liters','Other') NOT NULL DEFAULT 'Gallons',
  `price_per_unit` decimal(10,3) NOT NULL,
  `fuel_cost` decimal(10,2) NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_incident_response`
--

CREATE TABLE `vehicle_incident_response` (
  `incident_response_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `incident_id` int(11) NOT NULL,
  `response_type` enum('Investigation','Corrective Action','Report Filing','Escalation','Other') NOT NULL,
  `response_details` text NOT NULL,
  `response_team` text DEFAULT NULL,
  `outcome` enum('Resolved','Ongoing','Unresolved') DEFAULT 'Ongoing',
  `response_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_inspections`
--

CREATE TABLE `vehicle_inspections` (
  `inspection_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `inspection_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `inspection_type` enum('Routine','Accident','Safety','Other') NOT NULL,
  `inspector_name` varchar(255) NOT NULL,
  `inspection_result` text NOT NULL,
  `issues_found` text DEFAULT NULL,
  `next_inspection_due` date DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_inspections_history`
--

CREATE TABLE `vehicle_inspections_history` (
  `inspection_history_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `inspection_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `inspection_type` enum('Routine','Accident','Safety','Other') NOT NULL,
  `inspection_result` text NOT NULL,
  `issues_found` text DEFAULT NULL,
  `maintenance_required` text DEFAULT NULL,
  `next_inspection_due` date DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_inspection_history_log`
--

CREATE TABLE `vehicle_inspection_history_log` (
  `inspection_log_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `inspector_id` int(11) NOT NULL,
  `inspection_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `inspection_result` enum('Pass','Fail','Under Review') NOT NULL,
  `inspection_notes` text DEFAULT NULL,
  `follow_up_action` text DEFAULT NULL,
  `next_inspection_due` date DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_inspection_schedule`
--

CREATE TABLE `vehicle_inspection_schedule` (
  `inspection_schedule_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `inspection_type` enum('Routine','Safety','Accident','Emissions','Other') NOT NULL,
  `scheduled_date` date NOT NULL,
  `inspector_id` int(11) DEFAULT NULL,
  `inspection_status` enum('Scheduled','Completed','Postponed','Cancelled') DEFAULT 'Scheduled',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_insurance`
--

CREATE TABLE `vehicle_insurance` (
  `insurance_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `provider_name` varchar(255) NOT NULL,
  `policy_number` varchar(255) NOT NULL,
  `coverage_type` enum('Liability','Comprehensive','Collision','Full Coverage','Other') NOT NULL,
  `start_date` date NOT NULL,
  `end_date` date NOT NULL,
  `premium_amount` decimal(10,2) NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_insurance_claims`
--

CREATE TABLE `vehicle_insurance_claims` (
  `claim_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `claim_number` varchar(255) NOT NULL,
  `claim_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `claim_amount` decimal(10,2) NOT NULL,
  `claim_status` enum('Pending','Approved','Denied','Settled') DEFAULT 'Pending',
  `claim_description` text DEFAULT NULL,
  `incident_report_id` int(11) DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_insurance_expiry`
--

CREATE TABLE `vehicle_insurance_expiry` (
  `expiry_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `policy_number` varchar(255) NOT NULL,
  `expiry_date` date NOT NULL,
  `reminder_sent` enum('Yes','No') DEFAULT 'No',
  `reminder_date` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `renewal_status` enum('Pending','Completed','Expired') DEFAULT 'Pending',
  `renewal_fee` decimal(10,2) DEFAULT NULL,
  `renewal_notes` text DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_lease_agreements`
--

CREATE TABLE `vehicle_lease_agreements` (
  `lease_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `lessee_name` varchar(255) NOT NULL,
  `lease_start_date` date NOT NULL,
  `lease_end_date` date NOT NULL,
  `monthly_payment` decimal(10,2) NOT NULL,
  `lease_terms` text NOT NULL,
  `security_deposit` decimal(10,2) DEFAULT NULL,
  `renewal_option` enum('Yes','No') DEFAULT 'Yes',
  `starting_mileage` float(7,2) NOT NULL,
  `current_mileage` float(7,2) NOT NULL,
  `mileage_last_updated` timestamp NULL DEFAULT NULL,
  `allowed_annual_miles` float(9,2) NOT NULL DEFAULT 0.00,
  `total_allowed_miles` float(9,2) NOT NULL DEFAULT 0.00,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_licensing`
--

CREATE TABLE `vehicle_licensing` (
  `license_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `license_number` varchar(255) NOT NULL,
  `issue_date` date NOT NULL,
  `expiration_date` date NOT NULL,
  `issued_by` varchar(255) DEFAULT NULL,
  `status` enum('Active','Expired','Pending') DEFAULT 'Active',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_maintenance_logs`
--

CREATE TABLE `vehicle_maintenance_logs` (
  `maintenance_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `maintenance_type` enum('Oil Change','Tire Replacement','Engine Repair','Brake Service','Other') NOT NULL,
  `maintenance_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `service_details` text NOT NULL,
  `service_provider` varchar(255) NOT NULL,
  `next_service_date` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_mileage_history`
--

CREATE TABLE `vehicle_mileage_history` (
  `mileage_history_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `recorded_mileage` int(11) NOT NULL,
  `mileage_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `service_type` enum('Routine Check','Maintenance','Fueling','Repair','Other') NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_performance_monitoring`
--

CREATE TABLE `vehicle_performance_monitoring` (
  `performance_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `monitoring_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `speed` float(9,2) DEFAULT NULL,
  `fuel_efficiency` float(9,2) DEFAULT NULL,
  `engine_temperature` float(9,2) DEFAULT NULL,
  `tire_pressure` float(9,2) DEFAULT NULL,
  `diagnostic_report` text DEFAULT NULL,
  `overall_status` enum('Good','Fair','Poor','Critical') DEFAULT 'Good',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_registration`
--

CREATE TABLE `vehicle_registration` (
  `registration_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `registration_number` varchar(255) NOT NULL,
  `registration_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `expiration_date` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00',
  `issued_by` varchar(255) DEFAULT NULL,
  `status` enum('active','expired','pending') DEFAULT 'active',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_registration_expiry`
--

CREATE TABLE `vehicle_registration_expiry` (
  `expiry_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `registration_number` varchar(255) NOT NULL,
  `expiry_date` date NOT NULL,
  `reminder_sent` enum('Yes','No') DEFAULT 'No',
  `reminder_date` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `renewal_status` enum('Pending','Completed','Expired') DEFAULT 'Pending',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_registration_renewal`
--

CREATE TABLE `vehicle_registration_renewal` (
  `renewal_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `renewal_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `renewal_due_date` date NOT NULL,
  `renewal_status` enum('Pending','Completed','Expired') DEFAULT 'Pending',
  `renewal_fee` decimal(10,2) DEFAULT NULL,
  `renewal_notes` text DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_services`
--

CREATE TABLE `vehicle_services` (
  `service_id` int(11) NOT NULL,
  `service_name` varchar(255) NOT NULL,
  `service_description` text DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_service_history`
--

CREATE TABLE `vehicle_service_history` (
  `service_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `service_type` enum('Routine','Repair','Inspection','Other') NOT NULL,
  `service_description` text NOT NULL,
  `service_provider` varchar(255) NOT NULL,
  `service_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `service_cost` decimal(10,2) DEFAULT NULL,
  `next_service_due` date DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_service_provider`
--

CREATE TABLE `vehicle_service_provider` (
  `provider_id` int(11) NOT NULL,
  `provider_name` varchar(255) NOT NULL,
  `provider_type` enum('Repair Shop','Dealership','Service Center','Other') NOT NULL,
  `contact_name` varchar(255) DEFAULT NULL,
  `contact_phone` varchar(20) DEFAULT NULL,
  `contact_email` varchar(100) DEFAULT NULL,
  `service_address` varchar(255) DEFAULT NULL,
  `service_areas` text DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_service_provider_services`
--

CREATE TABLE `vehicle_service_provider_services` (
  `provider_id` int(11) NOT NULL,
  `service_id` int(11) NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_service_reminder`
--

CREATE TABLE `vehicle_service_reminder` (
  `reminder_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `service_type` enum('Oil Change','Tire Rotation','Brake Inspection','Engine Check','Other') NOT NULL,
  `reminder_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `due_date` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00',
  `reminder_status` enum('Pending','Completed','Overdue') DEFAULT 'Pending',
  `reminder_notes` text DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_service_schedule`
--

CREATE TABLE `vehicle_service_schedule` (
  `service_schedule_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `service_type` enum('Oil Change','Tire Rotation','Engine Check','Transmission Service','Other') NOT NULL,
  `scheduled_date` date NOT NULL,
  `service_provider` varchar(255) DEFAULT NULL,
  `service_status` enum('Scheduled','Completed','Cancelled') DEFAULT 'Scheduled',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_taxation`
--

CREATE TABLE `vehicle_taxation` (
  `taxation_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `tax_type` enum('Registration Tax','Road Tax','Fuel Tax','Other') NOT NULL,
  `tax_amount` decimal(10,2) NOT NULL,
  `tax_due_date` date NOT NULL,
  `tax_paid` enum('Yes','No') DEFAULT 'No',
  `paid_date` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_tire_history`
--

CREATE TABLE `vehicle_tire_history` (
  `tire_history_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `tire_type` enum('Standard','Winter','All-Season','Specialty') NOT NULL,
  `tire_size` varchar(50) NOT NULL,
  `replacement_date` timestamp NOT NULL DEFAULT current_timestamp(),
  `mileage_at_replacement` int(11) NOT NULL,
  `tire_condition` enum('New','Used','Damaged') DEFAULT 'New',
  `service_provider` varchar(255) NOT NULL,
  `cost` decimal(10,2) NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `vehicle_warranty`
--

CREATE TABLE `vehicle_warranty` (
  `warranty_id` int(11) NOT NULL,
  `vehicle_id` int(11) NOT NULL,
  `warranty_provider` varchar(255) NOT NULL,
  `warranty_type` enum('Basic','Extended','Powertrain','Other') NOT NULL,
  `coverage_details` text NOT NULL,
  `start_date` date NOT NULL,
  `end_date` date NOT NULL,
  `warranty_status` enum('Active','Expired','Pending') DEFAULT 'Active',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `worksites`
--

CREATE TABLE `worksites` (
  `worksite_id` int(11) NOT NULL,
  `worksite_name` varchar(255) DEFAULT NULL,
  `client_id` int(11) NOT NULL DEFAULT 0,
  `site_number` int(11) NOT NULL DEFAULT 0,
  `region_id` int(11) NOT NULL DEFAULT 0,
  `regional_supervisor` int(11) NOT NULL DEFAULT 0,
  `site_supervisor` int(11) NOT NULL DEFAULT 0,
  `contract_id` int(11) NOT NULL DEFAULT 0,
  `default_payscale` int(11) NOT NULL DEFAULT 0,
  `shifts_per_week` int(11) NOT NULL DEFAULT 1,
  `shift_officers` int(11) NOT NULL DEFAULT 1 COMMENT 'Officers required per shift',
  `auth_officers` int(11) NOT NULL DEFAULT 1 COMMENT 'Total officers authorized for this site',
  `site_description` text DEFAULT NULL,
  `address_line1` varchar(255) DEFAULT NULL,
  `address_line2` varchar(255) DEFAULT NULL,
  `city` varchar(100) DEFAULT NULL,
  `state` varchar(50) DEFAULT NULL,
  `zip_code` varchar(20) DEFAULT NULL,
  `country` varchar(50) DEFAULT NULL,
  `contact_name` varchar(100) DEFAULT NULL,
  `contact_email` varchar(100) DEFAULT NULL,
  `contact_phone` varchar(20) DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `worksite_policies`
--

CREATE TABLE `worksite_policies` (
  `policy_id` int(11) NOT NULL,
  `worksite_id` int(11) NOT NULL,
  `policy_name` varchar(255) NOT NULL,
  `policy_details` text NOT NULL,
  `effective_date` date NOT NULL,
  `expiration_date` date DEFAULT NULL,
  `created_by` int(11) NOT NULL,
  `category_id` int(11) DEFAULT 0,
  `replaces_policy_id` int(11) DEFAULT 0,
  `client_id` int(11) DEFAULT 0,
  `role_id` int(11) DEFAULT 0,
  `status` enum('Pending','Active','Inactive') DEFAULT 'Pending',
  `approved_by` int(11) DEFAULT 0,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

-- --------------------------------------------------------

--
-- Table structure for table `worksite_schedules`
--

CREATE TABLE `worksite_schedules` (
  `schedule_id` int(11) NOT NULL,
  `worksite_id` int(11) NOT NULL,
  `officer_id` int(11) NOT NULL,
  `payscale_id` int(11) DEFAULT 0,
  `scheduled_start` datetime NOT NULL,
  `scheduled_end` datetime NOT NULL,
  `shift_type` enum('1st Shift','2nd Shift','3rd Shift') DEFAULT NULL,
  `shift_supervisor_id` int(11) DEFAULT NULL,
  `status` enum('scheduled','completed','pending','cancelled') DEFAULT 'scheduled',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;

--
-- Indexes for dumped tables
--

--
-- Indexes for table `attendance`
--
ALTER TABLE `attendance`
  ADD PRIMARY KEY (`attendance_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `worksite_id` (`worksite_id`);

--
-- Indexes for table `audit_logs`
--
ALTER TABLE `audit_logs`
  ADD PRIMARY KEY (`log_id`),
  ADD KEY `user_id` (`user_id`);

--
-- Indexes for table `clients`
--
ALTER TABLE `clients`
  ADD PRIMARY KEY (`client_id`);

--
-- Indexes for table `companies`
--
ALTER TABLE `companies`
  ADD PRIMARY KEY (`company_id`);

--
-- Indexes for table `contracts`
--
ALTER TABLE `contracts`
  ADD PRIMARY KEY (`contract_id`),
  ADD KEY `company_id` (`company_id`),
  ADD KEY `client_id` (`client_id`),
  ADD KEY `worksite_id` (`worksite_id`);

--
-- Indexes for table `contract_amendments`
--
ALTER TABLE `contract_amendments`
  ADD PRIMARY KEY (`amendment_id`),
  ADD KEY `contract_id` (`contract_id`),
  ADD KEY `amended_by` (`amended_by`),
  ADD KEY `reviewed_by` (`reviewed_by`);

--
-- Indexes for table `current_payrates`
--
ALTER TABLE `current_payrates`
  ADD PRIMARY KEY (`rate_id`),
  ADD KEY `payscale_id` (`payscale_id`);

--
-- Indexes for table `daily_payroll`
--
ALTER TABLE `daily_payroll`
  ADD PRIMARY KEY (`payroll_id`),
  ADD KEY `user_id` (`user_id`),
  ADD KEY `worksite_id` (`worksite_id`),
  ADD KEY `payscale_id` (`payscale_id`);

--
-- Indexes for table `emergency_contacts`
--
ALTER TABLE `emergency_contacts`
  ADD PRIMARY KEY (`contact_id`),
  ADD KEY `officer_id` (`officer_id`);

--
-- Indexes for table `employee_performance_reviews`
--
ALTER TABLE `employee_performance_reviews`
  ADD PRIMARY KEY (`review_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `reviewer_id` (`reviewer_id`);

--
-- Indexes for table `employment_history`
--
ALTER TABLE `employment_history`
  ADD PRIMARY KEY (`history_id`),
  ADD KEY `user_id` (`user_id`),
  ADD KEY `company_id` (`company_id`),
  ADD KEY `client_id` (`client_id`),
  ADD KEY `worksite_id` (`worksite_id`);

--
-- Indexes for table `fueling_stations`
--
ALTER TABLE `fueling_stations`
  ADD PRIMARY KEY (`station_id`);

--
-- Indexes for table `incident_reports`
--
ALTER TABLE `incident_reports`
  ADD PRIMARY KEY (`incident_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `worksite_id` (`worksite_id`),
  ADD KEY `shift_id` (`shift_id`),
  ADD KEY `report_by` (`report_by`);

--
-- Indexes for table `incident_report_actions`
--
ALTER TABLE `incident_report_actions`
  ADD PRIMARY KEY (`action_id`),
  ADD KEY `incident_id` (`incident_id`),
  ADD KEY `action_taken_by` (`action_taken_by`);

--
-- Indexes for table `incident_responses`
--
ALTER TABLE `incident_responses`
  ADD PRIMARY KEY (`response_id`),
  ADD KEY `breach_id` (`breach_id`),
  ADD KEY `created_by` (`created_by`);

--
-- Indexes for table `leave_history`
--
ALTER TABLE `leave_history`
  ADD PRIMARY KEY (`history_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `supervisor_id` (`supervisor_id`);

--
-- Indexes for table `log_entries`
--
ALTER TABLE `log_entries`
  ADD PRIMARY KEY (`entry_id`),
  ADD KEY `log_id` (`log_id`);

--
-- Indexes for table `log_entry_categories`
--
ALTER TABLE `log_entry_categories`
  ADD PRIMARY KEY (`category_id`);

--
-- Indexes for table `log_entry_media`
--
ALTER TABLE `log_entry_media`
  ADD PRIMARY KEY (`media_id`),
  ADD KEY `log_entry_id` (`log_entry_id`);

--
-- Indexes for table `notifications`
--
ALTER TABLE `notifications`
  ADD PRIMARY KEY (`notification_id`),
  ADD KEY `user_id` (`user_id`),
  ADD KEY `notification_type_id` (`notification_type_id`);

--
-- Indexes for table `notification_types`
--
ALTER TABLE `notification_types`
  ADD PRIMARY KEY (`notification_type_id`);

--
-- Indexes for table `overtime_requests`
--
ALTER TABLE `overtime_requests`
  ADD PRIMARY KEY (`request_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `supervisor_id` (`supervisor_id`),
  ADD KEY `shift_id` (`shift_id`),
  ADD KEY `approved_by` (`approved_by`);

--
-- Indexes for table `payscales`
--
ALTER TABLE `payscales`
  ADD PRIMARY KEY (`payscale_id`);

--
-- Indexes for table `roles`
--
ALTER TABLE `roles`
  ADD PRIMARY KEY (`role_id`);

--
-- Indexes for table `scheduled_time_off`
--
ALTER TABLE `scheduled_time_off`
  ADD PRIMARY KEY (`time_off_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `supervisor_id` (`supervisor_id`);

--
-- Indexes for table `security_breaches`
--
ALTER TABLE `security_breaches`
  ADD PRIMARY KEY (`breach_id`),
  ADD KEY `worksite_id` (`worksite_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `reported_by` (`reported_by`),
  ADD KEY `resolved_by` (`resolved_by`);

--
-- Indexes for table `shift_assignments`
--
ALTER TABLE `shift_assignments`
  ADD PRIMARY KEY (`assignment_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `worksite_id` (`worksite_id`),
  ADD KEY `shift_supervisor` (`shift_supervisor`);

--
-- Indexes for table `shift_changes`
--
ALTER TABLE `shift_changes`
  ADD PRIMARY KEY (`change_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `original_shift_id` (`original_shift_id`),
  ADD KEY `new_shift_id` (`new_shift_id`),
  ADD KEY `approved_by` (`approved_by`);

--
-- Indexes for table `shift_logs`
--
ALTER TABLE `shift_logs`
  ADD PRIMARY KEY (`log_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `shift_id` (`shift_id`);

--
-- Indexes for table `shift_reports`
--
ALTER TABLE `shift_reports`
  ADD PRIMARY KEY (`report_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `worksite_id` (`worksite_id`),
  ADD KEY `regional_supervisor_id` (`regional_supervisor_id`),
  ADD KEY `site_supervisor_id` (`site_supervisor_id`),
  ADD KEY `shift_supervisor_id` (`shift_supervisor_id`),
  ADD KEY `liaison_id` (`liaison_id`);

--
-- Indexes for table `shift_swaps`
--
ALTER TABLE `shift_swaps`
  ADD PRIMARY KEY (`swap_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `original_shift_id` (`original_shift_id`),
  ADD KEY `swap_with_officer_id` (`swap_with_officer_id`),
  ADD KEY `approved_by` (`approved_by`);

--
-- Indexes for table `system_settings`
--
ALTER TABLE `system_settings`
  ADD PRIMARY KEY (`setting_id`);

--
-- Indexes for table `task_assignments`
--
ALTER TABLE `task_assignments`
  ADD PRIMARY KEY (`task_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `assigned_by` (`assigned_by`);

--
-- Indexes for table `tax_rates`
--
ALTER TABLE `tax_rates`
  ADD PRIMARY KEY (`tax_rate_id`);

--
-- Indexes for table `time_off_requests`
--
ALTER TABLE `time_off_requests`
  ADD PRIMARY KEY (`request_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `approved_by` (`approved_by`),
  ADD KEY `covered_by` (`covered_by`);

--
-- Indexes for table `training_sessions`
--
ALTER TABLE `training_sessions`
  ADD PRIMARY KEY (`training_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `trainer_id` (`trainer_id`);

--
-- Indexes for table `users`
--
ALTER TABLE `users`
  ADD PRIMARY KEY (`user_id`),
  ADD UNIQUE KEY `username` (`username`),
  ADD UNIQUE KEY `email` (`email`),
  ADD KEY `current_company` (`current_company`),
  ADD KEY `current_client` (`current_client`),
  ADD KEY `current_worksite` (`current_worksite`);

--
-- Indexes for table `vehicle_accident_history`
--
ALTER TABLE `vehicle_accident_history`
  ADD PRIMARY KEY (`accident_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_assignments`
--
ALTER TABLE `vehicle_assignments`
  ADD PRIMARY KEY (`assignment_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_damage_claims`
--
ALTER TABLE `vehicle_damage_claims`
  ADD PRIMARY KEY (`damage_claim_id`),
  ADD KEY `vehicle_id` (`vehicle_id`),
  ADD KEY `incident_report_id` (`incident_report_id`);

--
-- Indexes for table `vehicle_damage_history`
--
ALTER TABLE `vehicle_damage_history`
  ADD PRIMARY KEY (`damage_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_fleet_management`
--
ALTER TABLE `vehicle_fleet_management`
  ADD PRIMARY KEY (`fleet_id`),
  ADD KEY `vehicle_id` (`vehicle_id`),
  ADD KEY `assigned_to` (`assigned_to`),
  ADD KEY `worksite_id` (`worksite_id`);

--
-- Indexes for table `vehicle_fuel_purchase`
--
ALTER TABLE `vehicle_fuel_purchase`
  ADD PRIMARY KEY (`fuel_entry_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_incident_response`
--
ALTER TABLE `vehicle_incident_response`
  ADD PRIMARY KEY (`incident_response_id`),
  ADD KEY `vehicle_id` (`vehicle_id`),
  ADD KEY `incident_id` (`incident_id`);

--
-- Indexes for table `vehicle_inspections`
--
ALTER TABLE `vehicle_inspections`
  ADD PRIMARY KEY (`inspection_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_inspections_history`
--
ALTER TABLE `vehicle_inspections_history`
  ADD PRIMARY KEY (`inspection_history_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_inspection_history_log`
--
ALTER TABLE `vehicle_inspection_history_log`
  ADD PRIMARY KEY (`inspection_log_id`),
  ADD KEY `vehicle_id` (`vehicle_id`),
  ADD KEY `inspector_id` (`inspector_id`);

--
-- Indexes for table `vehicle_inspection_schedule`
--
ALTER TABLE `vehicle_inspection_schedule`
  ADD PRIMARY KEY (`inspection_schedule_id`),
  ADD KEY `vehicle_id` (`vehicle_id`),
  ADD KEY `inspector_id` (`inspector_id`);

--
-- Indexes for table `vehicle_insurance`
--
ALTER TABLE `vehicle_insurance`
  ADD PRIMARY KEY (`insurance_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_insurance_claims`
--
ALTER TABLE `vehicle_insurance_claims`
  ADD PRIMARY KEY (`claim_id`),
  ADD KEY `vehicle_id` (`vehicle_id`),
  ADD KEY `incident_report_id` (`incident_report_id`);

--
-- Indexes for table `vehicle_insurance_expiry`
--
ALTER TABLE `vehicle_insurance_expiry`
  ADD PRIMARY KEY (`expiry_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_lease_agreements`
--
ALTER TABLE `vehicle_lease_agreements`
  ADD PRIMARY KEY (`lease_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_licensing`
--
ALTER TABLE `vehicle_licensing`
  ADD PRIMARY KEY (`license_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_maintenance_logs`
--
ALTER TABLE `vehicle_maintenance_logs`
  ADD PRIMARY KEY (`maintenance_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_mileage_history`
--
ALTER TABLE `vehicle_mileage_history`
  ADD PRIMARY KEY (`mileage_history_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_performance_monitoring`
--
ALTER TABLE `vehicle_performance_monitoring`
  ADD PRIMARY KEY (`performance_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_registration`
--
ALTER TABLE `vehicle_registration`
  ADD PRIMARY KEY (`registration_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_registration_expiry`
--
ALTER TABLE `vehicle_registration_expiry`
  ADD PRIMARY KEY (`expiry_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_registration_renewal`
--
ALTER TABLE `vehicle_registration_renewal`
  ADD PRIMARY KEY (`renewal_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_services`
--
ALTER TABLE `vehicle_services`
  ADD PRIMARY KEY (`service_id`);

--
-- Indexes for table `vehicle_service_history`
--
ALTER TABLE `vehicle_service_history`
  ADD PRIMARY KEY (`service_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_service_provider`
--
ALTER TABLE `vehicle_service_provider`
  ADD PRIMARY KEY (`provider_id`);

--
-- Indexes for table `vehicle_service_provider_services`
--
ALTER TABLE `vehicle_service_provider_services`
  ADD PRIMARY KEY (`provider_id`,`service_id`),
  ADD KEY `service_id` (`service_id`);

--
-- Indexes for table `vehicle_service_reminder`
--
ALTER TABLE `vehicle_service_reminder`
  ADD PRIMARY KEY (`reminder_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_service_schedule`
--
ALTER TABLE `vehicle_service_schedule`
  ADD PRIMARY KEY (`service_schedule_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_taxation`
--
ALTER TABLE `vehicle_taxation`
  ADD PRIMARY KEY (`taxation_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_tire_history`
--
ALTER TABLE `vehicle_tire_history`
  ADD PRIMARY KEY (`tire_history_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `vehicle_warranty`
--
ALTER TABLE `vehicle_warranty`
  ADD PRIMARY KEY (`warranty_id`),
  ADD KEY `vehicle_id` (`vehicle_id`);

--
-- Indexes for table `worksites`
--
ALTER TABLE `worksites`
  ADD PRIMARY KEY (`worksite_id`),
  ADD KEY `client_id` (`client_id`);

--
-- Indexes for table `worksite_policies`
--
ALTER TABLE `worksite_policies`
  ADD PRIMARY KEY (`policy_id`),
  ADD KEY `worksite_id` (`worksite_id`),
  ADD KEY `created_by` (`created_by`),
  ADD KEY `approved_by` (`approved_by`);

--
-- Indexes for table `worksite_schedules`
--
ALTER TABLE `worksite_schedules`
  ADD PRIMARY KEY (`schedule_id`),
  ADD KEY `worksite_id` (`worksite_id`),
  ADD KEY `officer_id` (`officer_id`),
  ADD KEY `payscale_id` (`payscale_id`),
  ADD KEY `shift_supervisor_id` (`shift_supervisor_id`);

--
-- AUTO_INCREMENT for dumped tables
--

--
-- AUTO_INCREMENT for table `attendance`
--
ALTER TABLE `attendance`
  MODIFY `attendance_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `audit_logs`
--
ALTER TABLE `audit_logs`
  MODIFY `log_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `clients`
--
ALTER TABLE `clients`
  MODIFY `client_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `companies`
--
ALTER TABLE `companies`
  MODIFY `company_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `contracts`
--
ALTER TABLE `contracts`
  MODIFY `contract_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `contract_amendments`
--
ALTER TABLE `contract_amendments`
  MODIFY `amendment_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `current_payrates`
--
ALTER TABLE `current_payrates`
  MODIFY `rate_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `daily_payroll`
--
ALTER TABLE `daily_payroll`
  MODIFY `payroll_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `emergency_contacts`
--
ALTER TABLE `emergency_contacts`
  MODIFY `contact_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `employee_performance_reviews`
--
ALTER TABLE `employee_performance_reviews`
  MODIFY `review_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `employment_history`
--
ALTER TABLE `employment_history`
  MODIFY `history_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `fueling_stations`
--
ALTER TABLE `fueling_stations`
  MODIFY `station_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `incident_reports`
--
ALTER TABLE `incident_reports`
  MODIFY `incident_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `incident_report_actions`
--
ALTER TABLE `incident_report_actions`
  MODIFY `action_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `incident_responses`
--
ALTER TABLE `incident_responses`
  MODIFY `response_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `leave_history`
--
ALTER TABLE `leave_history`
  MODIFY `history_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `log_entries`
--
ALTER TABLE `log_entries`
  MODIFY `entry_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `log_entry_categories`
--
ALTER TABLE `log_entry_categories`
  MODIFY `category_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `log_entry_media`
--
ALTER TABLE `log_entry_media`
  MODIFY `media_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `notifications`
--
ALTER TABLE `notifications`
  MODIFY `notification_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `notification_types`
--
ALTER TABLE `notification_types`
  MODIFY `notification_type_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `overtime_requests`
--
ALTER TABLE `overtime_requests`
  MODIFY `request_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `payscales`
--
ALTER TABLE `payscales`
  MODIFY `payscale_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `roles`
--
ALTER TABLE `roles`
  MODIFY `role_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `scheduled_time_off`
--
ALTER TABLE `scheduled_time_off`
  MODIFY `time_off_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `security_breaches`
--
ALTER TABLE `security_breaches`
  MODIFY `breach_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `shift_assignments`
--
ALTER TABLE `shift_assignments`
  MODIFY `assignment_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `shift_changes`
--
ALTER TABLE `shift_changes`
  MODIFY `change_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `shift_logs`
--
ALTER TABLE `shift_logs`
  MODIFY `log_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `shift_reports`
--
ALTER TABLE `shift_reports`
  MODIFY `report_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `shift_swaps`
--
ALTER TABLE `shift_swaps`
  MODIFY `swap_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `system_settings`
--
ALTER TABLE `system_settings`
  MODIFY `setting_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `task_assignments`
--
ALTER TABLE `task_assignments`
  MODIFY `task_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `tax_rates`
--
ALTER TABLE `tax_rates`
  MODIFY `tax_rate_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `time_off_requests`
--
ALTER TABLE `time_off_requests`
  MODIFY `request_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `training_sessions`
--
ALTER TABLE `training_sessions`
  MODIFY `training_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `users`
--
ALTER TABLE `users`
  MODIFY `user_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_accident_history`
--
ALTER TABLE `vehicle_accident_history`
  MODIFY `accident_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_assignments`
--
ALTER TABLE `vehicle_assignments`
  MODIFY `assignment_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_damage_claims`
--
ALTER TABLE `vehicle_damage_claims`
  MODIFY `damage_claim_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_damage_history`
--
ALTER TABLE `vehicle_damage_history`
  MODIFY `damage_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_fleet_management`
--
ALTER TABLE `vehicle_fleet_management`
  MODIFY `fleet_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_fuel_purchase`
--
ALTER TABLE `vehicle_fuel_purchase`
  MODIFY `fuel_entry_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_incident_response`
--
ALTER TABLE `vehicle_incident_response`
  MODIFY `incident_response_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_inspections`
--
ALTER TABLE `vehicle_inspections`
  MODIFY `inspection_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_inspections_history`
--
ALTER TABLE `vehicle_inspections_history`
  MODIFY `inspection_history_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_inspection_history_log`
--
ALTER TABLE `vehicle_inspection_history_log`
  MODIFY `inspection_log_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_inspection_schedule`
--
ALTER TABLE `vehicle_inspection_schedule`
  MODIFY `inspection_schedule_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_insurance`
--
ALTER TABLE `vehicle_insurance`
  MODIFY `insurance_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_insurance_claims`
--
ALTER TABLE `vehicle_insurance_claims`
  MODIFY `claim_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_insurance_expiry`
--
ALTER TABLE `vehicle_insurance_expiry`
  MODIFY `expiry_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_lease_agreements`
--
ALTER TABLE `vehicle_lease_agreements`
  MODIFY `lease_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_licensing`
--
ALTER TABLE `vehicle_licensing`
  MODIFY `license_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_maintenance_logs`
--
ALTER TABLE `vehicle_maintenance_logs`
  MODIFY `maintenance_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_mileage_history`
--
ALTER TABLE `vehicle_mileage_history`
  MODIFY `mileage_history_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_performance_monitoring`
--
ALTER TABLE `vehicle_performance_monitoring`
  MODIFY `performance_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_registration`
--
ALTER TABLE `vehicle_registration`
  MODIFY `registration_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_registration_expiry`
--
ALTER TABLE `vehicle_registration_expiry`
  MODIFY `expiry_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_registration_renewal`
--
ALTER TABLE `vehicle_registration_renewal`
  MODIFY `renewal_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_services`
--
ALTER TABLE `vehicle_services`
  MODIFY `service_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_service_history`
--
ALTER TABLE `vehicle_service_history`
  MODIFY `service_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_service_provider`
--
ALTER TABLE `vehicle_service_provider`
  MODIFY `provider_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_service_reminder`
--
ALTER TABLE `vehicle_service_reminder`
  MODIFY `reminder_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_service_schedule`
--
ALTER TABLE `vehicle_service_schedule`
  MODIFY `service_schedule_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_taxation`
--
ALTER TABLE `vehicle_taxation`
  MODIFY `taxation_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_tire_history`
--
ALTER TABLE `vehicle_tire_history`
  MODIFY `tire_history_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `vehicle_warranty`
--
ALTER TABLE `vehicle_warranty`
  MODIFY `warranty_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `worksites`
--
ALTER TABLE `worksites`
  MODIFY `worksite_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `worksite_policies`
--
ALTER TABLE `worksite_policies`
  MODIFY `policy_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT for table `worksite_schedules`
--
ALTER TABLE `worksite_schedules`
  MODIFY `schedule_id` int(11) NOT NULL AUTO_INCREMENT;

--
-- Constraints for dumped tables
--

--
-- Constraints for table `attendance`
--
ALTER TABLE `attendance`
  ADD CONSTRAINT `attendance_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `attendance_ibfk_2` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE;

--
-- Constraints for table `audit_logs`
--
ALTER TABLE `audit_logs`
  ADD CONSTRAINT `audit_logs_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `contracts`
--
ALTER TABLE `contracts`
  ADD CONSTRAINT `contracts_ibfk_1` FOREIGN KEY (`company_id`) REFERENCES `companies` (`company_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `contracts_ibfk_2` FOREIGN KEY (`client_id`) REFERENCES `clients` (`client_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `contracts_ibfk_3` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE;

--
-- Constraints for table `contract_amendments`
--
ALTER TABLE `contract_amendments`
  ADD CONSTRAINT `contract_amendments_ibfk_1` FOREIGN KEY (`contract_id`) REFERENCES `contracts` (`contract_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `contract_amendments_ibfk_2` FOREIGN KEY (`amended_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `contract_amendments_ibfk_3` FOREIGN KEY (`reviewed_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `current_payrates`
--
ALTER TABLE `current_payrates`
  ADD CONSTRAINT `current_payrates_ibfk_1` FOREIGN KEY (`payscale_id`) REFERENCES `payscales` (`payscale_id`) ON DELETE CASCADE;

--
-- Constraints for table `daily_payroll`
--
ALTER TABLE `daily_payroll`
  ADD CONSTRAINT `daily_payroll_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `daily_payroll_ibfk_2` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `daily_payroll_ibfk_3` FOREIGN KEY (`payscale_id`) REFERENCES `payscales` (`payscale_id`) ON DELETE CASCADE;

--
-- Constraints for table `emergency_contacts`
--
ALTER TABLE `emergency_contacts`
  ADD CONSTRAINT `emergency_contacts_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `employee_performance_reviews`
--
ALTER TABLE `employee_performance_reviews`
  ADD CONSTRAINT `employee_performance_reviews_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `employee_performance_reviews_ibfk_2` FOREIGN KEY (`reviewer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `employment_history`
--
ALTER TABLE `employment_history`
  ADD CONSTRAINT `employment_history_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `employment_history_ibfk_2` FOREIGN KEY (`company_id`) REFERENCES `companies` (`company_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `employment_history_ibfk_3` FOREIGN KEY (`client_id`) REFERENCES `clients` (`client_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `employment_history_ibfk_4` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE;

--
-- Constraints for table `incident_reports`
--
ALTER TABLE `incident_reports`
  ADD CONSTRAINT `incident_reports_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `incident_reports_ibfk_2` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `incident_reports_ibfk_3` FOREIGN KEY (`shift_id`) REFERENCES `shift_assignments` (`assignment_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `incident_reports_ibfk_4` FOREIGN KEY (`report_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `incident_report_actions`
--
ALTER TABLE `incident_report_actions`
  ADD CONSTRAINT `incident_report_actions_ibfk_1` FOREIGN KEY (`incident_id`) REFERENCES `security_breaches` (`breach_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `incident_report_actions_ibfk_2` FOREIGN KEY (`action_taken_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `incident_responses`
--
ALTER TABLE `incident_responses`
  ADD CONSTRAINT `incident_responses_ibfk_1` FOREIGN KEY (`breach_id`) REFERENCES `security_breaches` (`breach_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `incident_responses_ibfk_2` FOREIGN KEY (`created_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `leave_history`
--
ALTER TABLE `leave_history`
  ADD CONSTRAINT `leave_history_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `leave_history_ibfk_2` FOREIGN KEY (`supervisor_id`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `log_entries`
--
ALTER TABLE `log_entries`
  ADD CONSTRAINT `log_entries_ibfk_1` FOREIGN KEY (`log_id`) REFERENCES `shift_logs` (`log_id`) ON DELETE CASCADE;

--
-- Constraints for table `log_entry_media`
--
ALTER TABLE `log_entry_media`
  ADD CONSTRAINT `log_entry_media_ibfk_1` FOREIGN KEY (`log_entry_id`) REFERENCES `log_entries` (`entry_id`) ON DELETE CASCADE;

--
-- Constraints for table `notifications`
--
ALTER TABLE `notifications`
  ADD CONSTRAINT `notifications_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `notifications_ibfk_2` FOREIGN KEY (`notification_type_id`) REFERENCES `notification_types` (`notification_type_id`) ON DELETE CASCADE;

--
-- Constraints for table `overtime_requests`
--
ALTER TABLE `overtime_requests`
  ADD CONSTRAINT `overtime_requests_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `overtime_requests_ibfk_2` FOREIGN KEY (`supervisor_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `overtime_requests_ibfk_3` FOREIGN KEY (`shift_id`) REFERENCES `shift_assignments` (`assignment_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `overtime_requests_ibfk_4` FOREIGN KEY (`approved_by`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `scheduled_time_off`
--
ALTER TABLE `scheduled_time_off`
  ADD CONSTRAINT `scheduled_time_off_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `scheduled_time_off_ibfk_2` FOREIGN KEY (`supervisor_id`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `security_breaches`
--
ALTER TABLE `security_breaches`
  ADD CONSTRAINT `security_breaches_ibfk_1` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `security_breaches_ibfk_2` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `security_breaches_ibfk_3` FOREIGN KEY (`reported_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `security_breaches_ibfk_4` FOREIGN KEY (`resolved_by`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `shift_assignments`
--
ALTER TABLE `shift_assignments`
  ADD CONSTRAINT `shift_assignments_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_assignments_ibfk_2` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_assignments_ibfk_3` FOREIGN KEY (`shift_supervisor`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `shift_changes`
--
ALTER TABLE `shift_changes`
  ADD CONSTRAINT `shift_changes_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_changes_ibfk_2` FOREIGN KEY (`original_shift_id`) REFERENCES `shift_assignments` (`assignment_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_changes_ibfk_3` FOREIGN KEY (`new_shift_id`) REFERENCES `shift_assignments` (`assignment_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_changes_ibfk_4` FOREIGN KEY (`approved_by`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `shift_logs`
--
ALTER TABLE `shift_logs`
  ADD CONSTRAINT `shift_logs_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_logs_ibfk_2` FOREIGN KEY (`shift_id`) REFERENCES `shift_assignments` (`assignment_id`) ON DELETE CASCADE;

--
-- Constraints for table `shift_reports`
--
ALTER TABLE `shift_reports`
  ADD CONSTRAINT `shift_reports_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_reports_ibfk_2` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_reports_ibfk_3` FOREIGN KEY (`regional_supervisor_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_reports_ibfk_4` FOREIGN KEY (`site_supervisor_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_reports_ibfk_5` FOREIGN KEY (`shift_supervisor_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_reports_ibfk_6` FOREIGN KEY (`liaison_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `shift_swaps`
--
ALTER TABLE `shift_swaps`
  ADD CONSTRAINT `shift_swaps_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_swaps_ibfk_2` FOREIGN KEY (`original_shift_id`) REFERENCES `shift_assignments` (`assignment_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_swaps_ibfk_3` FOREIGN KEY (`swap_with_officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_swaps_ibfk_4` FOREIGN KEY (`approved_by`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `task_assignments`
--
ALTER TABLE `task_assignments`
  ADD CONSTRAINT `task_assignments_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `task_assignments_ibfk_2` FOREIGN KEY (`assigned_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `time_off_requests`
--
ALTER TABLE `time_off_requests`
  ADD CONSTRAINT `time_off_requests_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `time_off_requests_ibfk_2` FOREIGN KEY (`approved_by`) REFERENCES `users` (`user_id`) ON DELETE SET NULL,
  ADD CONSTRAINT `time_off_requests_ibfk_3` FOREIGN KEY (`covered_by`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `training_sessions`
--
ALTER TABLE `training_sessions`
  ADD CONSTRAINT `training_sessions_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `training_sessions_ibfk_2` FOREIGN KEY (`trainer_id`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `users`
--
ALTER TABLE `users`
  ADD CONSTRAINT `users_ibfk_1` FOREIGN KEY (`current_company`) REFERENCES `companies` (`company_id`) ON DELETE SET NULL,
  ADD CONSTRAINT `users_ibfk_2` FOREIGN KEY (`current_client`) REFERENCES `clients` (`client_id`) ON DELETE SET NULL,
  ADD CONSTRAINT `users_ibfk_3` FOREIGN KEY (`current_worksite`) REFERENCES `worksites` (`worksite_id`) ON DELETE SET NULL;

--
-- Constraints for table `vehicle_accident_history`
--
ALTER TABLE `vehicle_accident_history`
  ADD CONSTRAINT `vehicle_accident_history_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_assignments`
--
ALTER TABLE `vehicle_assignments`
  ADD CONSTRAINT `vehicle_assignments_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_assignments_ibfk_2` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_damage_claims`
--
ALTER TABLE `vehicle_damage_claims`
  ADD CONSTRAINT `vehicle_damage_claims_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_damage_claims_ibfk_2` FOREIGN KEY (`incident_report_id`) REFERENCES `security_breaches` (`breach_id`) ON DELETE SET NULL;

--
-- Constraints for table `vehicle_damage_history`
--
ALTER TABLE `vehicle_damage_history`
  ADD CONSTRAINT `vehicle_damage_history_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_fleet_management`
--
ALTER TABLE `vehicle_fleet_management`
  ADD CONSTRAINT `vehicle_fleet_management_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_fleet_management_ibfk_2` FOREIGN KEY (`assigned_to`) REFERENCES `users` (`user_id`) ON DELETE SET NULL,
  ADD CONSTRAINT `vehicle_fleet_management_ibfk_3` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE SET NULL;

--
-- Constraints for table `vehicle_fuel_purchase`
--
ALTER TABLE `vehicle_fuel_purchase`
  ADD CONSTRAINT `vehicle_fuel_purchase_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_incident_response`
--
ALTER TABLE `vehicle_incident_response`
  ADD CONSTRAINT `vehicle_incident_response_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_incident_response_ibfk_2` FOREIGN KEY (`incident_id`) REFERENCES `security_breaches` (`breach_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_inspections`
--
ALTER TABLE `vehicle_inspections`
  ADD CONSTRAINT `vehicle_inspections_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_inspections_history`
--
ALTER TABLE `vehicle_inspections_history`
  ADD CONSTRAINT `vehicle_inspections_history_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_inspection_history_log`
--
ALTER TABLE `vehicle_inspection_history_log`
  ADD CONSTRAINT `vehicle_inspection_history_log_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_inspection_history_log_ibfk_2` FOREIGN KEY (`inspector_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_inspection_schedule`
--
ALTER TABLE `vehicle_inspection_schedule`
  ADD CONSTRAINT `vehicle_inspection_schedule_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_inspection_schedule_ibfk_2` FOREIGN KEY (`inspector_id`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `vehicle_insurance`
--
ALTER TABLE `vehicle_insurance`
  ADD CONSTRAINT `vehicle_insurance_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_insurance_claims`
--
ALTER TABLE `vehicle_insurance_claims`
  ADD CONSTRAINT `vehicle_insurance_claims_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_insurance_claims_ibfk_2` FOREIGN KEY (`incident_report_id`) REFERENCES `security_breaches` (`breach_id`) ON DELETE SET NULL;

--
-- Constraints for table `vehicle_insurance_expiry`
--
ALTER TABLE `vehicle_insurance_expiry`
  ADD CONSTRAINT `vehicle_insurance_expiry_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_lease_agreements`
--
ALTER TABLE `vehicle_lease_agreements`
  ADD CONSTRAINT `vehicle_lease_agreements_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_licensing`
--
ALTER TABLE `vehicle_licensing`
  ADD CONSTRAINT `vehicle_licensing_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_maintenance_logs`
--
ALTER TABLE `vehicle_maintenance_logs`
  ADD CONSTRAINT `vehicle_maintenance_logs_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_mileage_history`
--
ALTER TABLE `vehicle_mileage_history`
  ADD CONSTRAINT `vehicle_mileage_history_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_performance_monitoring`
--
ALTER TABLE `vehicle_performance_monitoring`
  ADD CONSTRAINT `vehicle_performance_monitoring_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_registration`
--
ALTER TABLE `vehicle_registration`
  ADD CONSTRAINT `vehicle_registration_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_registration_expiry`
--
ALTER TABLE `vehicle_registration_expiry`
  ADD CONSTRAINT `vehicle_registration_expiry_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_registration_renewal`
--
ALTER TABLE `vehicle_registration_renewal`
  ADD CONSTRAINT `vehicle_registration_renewal_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_service_history`
--
ALTER TABLE `vehicle_service_history`
  ADD CONSTRAINT `vehicle_service_history_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_service_provider_services`
--
ALTER TABLE `vehicle_service_provider_services`
  ADD CONSTRAINT `vehicle_service_provider_services_ibfk_1` FOREIGN KEY (`provider_id`) REFERENCES `vehicle_service_provider` (`provider_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_service_provider_services_ibfk_2` FOREIGN KEY (`service_id`) REFERENCES `vehicle_services` (`service_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_service_reminder`
--
ALTER TABLE `vehicle_service_reminder`
  ADD CONSTRAINT `vehicle_service_reminder_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_service_schedule`
--
ALTER TABLE `vehicle_service_schedule`
  ADD CONSTRAINT `vehicle_service_schedule_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_taxation`
--
ALTER TABLE `vehicle_taxation`
  ADD CONSTRAINT `vehicle_taxation_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_tire_history`
--
ALTER TABLE `vehicle_tire_history`
  ADD CONSTRAINT `vehicle_tire_history_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_warranty`
--
ALTER TABLE `vehicle_warranty`
  ADD CONSTRAINT `vehicle_warranty_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `worksites`
--
ALTER TABLE `worksites`
  ADD CONSTRAINT `worksites_ibfk_1` FOREIGN KEY (`client_id`) REFERENCES `clients` (`client_id`) ON DELETE CASCADE;

--
-- Constraints for table `worksite_policies`
--
ALTER TABLE `worksite_policies`
  ADD CONSTRAINT `worksite_policies_ibfk_1` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `worksite_policies_ibfk_2` FOREIGN KEY (`created_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `worksite_policies_ibfk_3` FOREIGN KEY (`approved_by`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `worksite_schedules`
--
ALTER TABLE `worksite_schedules`
  ADD CONSTRAINT `worksite_schedules_ibfk_1` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `worksite_schedules_ibfk_2` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `worksite_schedules_ibfk_3` FOREIGN KEY (`payscale_id`) REFERENCES `payscales` (`payscale_id`) ON DELETE SET NULL,
  ADD CONSTRAINT `worksite_schedules_ibfk_4` FOREIGN KEY (`shift_supervisor_id`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;
COMMIT;

/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
